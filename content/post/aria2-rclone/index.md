+++
title = 'Aria2-pro 杀后台严重及替代方案'
date = 2024-07-23T21:18:05+08:00
categories = [
    "Rclone",
    "Aria2",
]
tags = [
    "BT",
    "Magnet",
]
image = "cover.jpg"
+++

## 前情提要

之前在网上看到 [Aria2-pro](https://github.com/P3TERX/Aria2-Pro-Docker) 配合 [rclone](https://rclone.org/)  可以实现服务器下载BT与Magnet等多种格式文件并自动上传云盘，于是我便通过docker部署了一下，结果发现没运行几分钟就自动关闭了，看了下别人的提问也没有收获

[无限重启 · Issue #99 · P3TERX/Aria2-Pro-Docker · GitHub](https://github.com/P3TERX/Aria2-Pro-Docker/issues/99)

之后呢，我克隆了最新的[配置文件](https://github.com/P3TERX/aria2.conf/blob/master/aria2.conf)来替代，却依然没有得到解决QwQ，于是便搜寻其他的 aria2 镜像来替代

## 替代方案

### docker 镜像

[GitHub - SuperNG6/docker-aria2: Docker Aria2的最佳实践](https://github.com/SuperNG6/docker-aria2)

直到我找到这个镜像，问题才得以解决，按照说明文档部署就行，不过这个项目并没有结合 rclone，也就无法上传云盘，于是我便编写了一个 rclone 自动上传脚本（使用前记得先安装 [rclone](https://rclone.org/) 并配置 remote）

### rclone 自动上传脚本

```bash
#!/bin/bash

# 配置变量
DOWNLOAD_DIR="your_download_path"
REMOTE_DIR="your_remote:/"
INTERVAL=3  # 检查间隔，单位为秒

# 检查下载目录是否存在
if [ ! -d "$DOWNLOAD_DIR" ]; then
  echo "下载目录不存在：$DOWNLOAD_DIR"
  exit 1
fi

# 上传文件和目录函数
upload_files() {
  for file in "$DOWNLOAD_DIR"/*; do
    # 忽略特定目录
    case "$(basename "$file")" in
      completed|recycle)
        echo "忽略目录：$file"
        ;;
      *)
        if [ -f "$file" ]; then
          case "$file" in
            *.aria2*|*.torrent)
              echo "忽略aria2/torrent文件：$file"
              ;;
            *)
              echo "上传文件：$file"
              rclone move "$file" "$REMOTE_DIR"
              if [ $? -eq 0 ]; then
                echo "成功上传并删除文件：$file"
              else
                echo "上传失败：$file"
              fi
              ;;
          esac
        elif [ -d "$file" ]; then
          echo "上传目录：$file"
          rclone move "$file" "$REMOTE_DIR"
          if [ $? -eq 0 ]; then
            echo "成功上传并删除目录：$file"
          else
            echo "上传失败：$file"
          fi
        fi
        ;;
    esac
  done
}

# 无限循环监听下载目录
while true; do
  echo "检查下载目录：$DOWNLOAD_DIR"
  upload_files
  echo "等待 $INTERVAL 秒后再次检查..."
  sleep $INTERVAL
done
```

这个脚本会每隔3秒自动遍历下载目录，检查是否有``*.aria2*``  ``*.torrent`` ``recycle`` ``completed`` 以外的文件与目录，若有则会自动上传到云盘，上传完成后则自动删除服务器的本地文件（有个特性就是rclone上传过的目录会把里面的文件遍历删掉，目录却没删，由于空目录占不了多少空间，我也没当回事儿OwO）

你需要更改的有两个地方

``DOWNLOAD_DIR`` 你文件的下载目录

``REMOTE_DIR`` 你remote的名称（``:/``表示存放到云盘的根目录）

之后保存以``.sh`` 为后缀的文件并赋予执行权限

```bash
chmod +x upload-dirve.sh
screen -S upload
./upload-dirve.sh
```

输入 ``ctrl + a + d`` 在screen后台窗口运行即可w
