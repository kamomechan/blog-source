+++
title = 'Matrix Sticker Packs'
date = 2024-06-20T21:38:08+08:00
categories = [
    "Matrix",
]
tags = [
    "Stickers",
]
image = "cover.jpg"
+++



你可以按照 [项目地址](https://github.com/maunium/stickerpicker) 创建属于自己的贴纸包，如果你觉得繁琐可以使用我创建的，希望你能喜欢，Matrix 群组贴纸需要在网页端添加，首先在群组中输入 /devtools 回车，点击 Explore account data 选项，接着点击 Send custom account data event选项，可以看见两个输入框，分别输入 m.widgets 与

```json
{
    "stickerpicker": {
        "content": {
            "type": "m.stickerpicker",
            "url": "https://sticker.tia-chan.top/web/?theme=$theme",
            "name": "Stickerpicker",
            "creatorUserId": "@you:matrix.server.name",
            "data": {}
        },
        "sender": "@you:matrix.server.name",
        "state_key": "stickerpicker",
        "type": "m.widget",
        "id": "stickerpicker"
    }
}
```

最后 send 就可以在sticker中看见啦ww

###### .
