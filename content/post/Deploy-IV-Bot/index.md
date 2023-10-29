+++
title = 'Deploy Telegram Instant View Bot'
date = 2023-10-29T20:20:37+08:00
image = "cover.png"
categories = [
    "Telegram Bot"
]
tags = [
    "Telegram Bot",
    "Instant View"
]
+++



按照 [项目地址](https://github.com/albertincx/formatbot1/) 的 Wiki 部署可能会遇到一些问题，所以在这写了一篇，希望能够帮助大家www

## 下载安装

### 克隆仓库

```shell
git clone  https://github.com/albertincx/formatbot1.git
cd formatbot1
```

### 安装 yarn

```shell
#移除cmdtest
sudo apt remove cmdtest
sudo apt remove yarn

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt update
sudo apt install yarn
```

### 安装模块和开发模块

```shell
yarn
bash dev-install.txt
```

## 配置

### 设置 Bot Token 与 User ID

创建并编辑配置文件

```shell
cp .env.example ./.env
vim .env
```

通过 [@BotFather](https://t.me/BotFather) 创建机器人并获取 Token

通过 [@userinfobot](https://t.me/userinfobot) 获取ID

```
TBTKN=你的Token
TGADMIN=你的Telegram ID
```

### 设置 Telegraph token

通过 [Telegraph API – Telegraph](https://telegra.ph/api) 获取 access_token

```
TGPHTOKEN_0=telegra.ph token 
TGPHTOKEN_1=telegra.ph token 
...
TGPHTOKEN_6=telegra.ph token 
```

### 设置 URL

通过 [cloudamqp](https://cloudamqp.com/) 获取 URL

```
MESSAGE_QUEUE=来自cloudamqp的url
```

### 安装 chrome

不安装 chrome 运行时会报错

下载

```shell
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

安装

```shell
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

如果安装过程中报错，执行下方命令来修复

```shell
apt-get install -f
```

## 运行

```shell
yarn dev or PORT=CUSTOM_PORT yarn dev
```
