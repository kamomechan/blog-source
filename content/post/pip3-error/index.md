+++
title = 'pip3 install 报错的解决方案'
date = 2023-10-05T12:46:19+08:00
categories = [
    "Python"
]
tags = [
    "Python",
    "pip"
]
image = "1.png"
+++

## 报错信息：

```subunit
error: externally-managed-environment
```

## 解决方案：

```awk
sudo mv /usr/lib/python3.11/EXTERNALLY-MANAGED /usr/lib/python3.11/EXTERNALLY-MANAGED.old
```

使用这个命令传递错误就可以啦！

## 碎碎念

呜呜呜呜呜，一开始遇到这个错误，找到半天解决方案都不行！！~~被打~~

最后在这里才找到办法www：[link](https://stackoverflow.com/questions/75608323/how-do-i-solve-error-externally-managed-environment-every-time-i-use-pip-3 "link")
