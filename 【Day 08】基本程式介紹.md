
# 【Day 08】基本程式介紹
###### tags: iT鐵人邦
我們回到第三天建立專案那天寫的**echo**程式碼。
結果如下：
![](https://i.imgur.com/0Jv2cMc.png)

## 主程式
### Text Message
我們再把這個程式碼打出來 
```python=
def callback(request):
    if request.method == 'POST':
        signature = request.META['HTTP_X_LINE_SIGNATURE']
        body = request.body.decode('utf-8')

        try:
            events = parser.parse(body, signature)  # 傳入的事件
        except InvalidSignatureError:
            return HttpResponseForbidden()
        except LineBotApiError:
            return HttpResponseBadRequest()

        for event in events:
            if isinstance(event, MessageEvent):  # 如果有訊息事件
                line_bot_api.reply_message(  # 回復傳入的訊息文字
                    event.reply_token,
                    TextSendMessage(text=event.message.text)
                )
        return HttpResponse()
    else:
        return HttpResponseBadRequest()
```

我們專注在 `Line 13` 開始

```python=13
for event in events:
    if isinstance(event, MessageEvent):  # 如果有訊息事件
        line_bot_api.reply_message(  # 回復傳入的訊息文字
            event.reply_token,
            TextSendMessage(text=event.message.text)
        )
```

昨天有提到，`event`即是有**事件**傳入。
有多少種事件？昨天都有講過了～。

```python=14
if isinstance(event, MessageEvent):
```

在這裡先來講講 `isinstance` 的作用～

### `isinstance`

這個 python funtion 類似 `type()`，

兩者區別再於：
+ `type()` 不會考慮繼承關係（`class` 裡的`Inheritance`），他不會認為子類別（Sub Class）是父類別（Base Class）的一種。
+ `isinstance()` 會考慮繼承關係，所以 Sub Class 會被認為跟 Base Class 同類別。

```python=14
if isinstance(event, MessageEvent):
    line_bot_api.reply_message(  # 回復傳入的訊息文字
        event.reply_token,
        TextSendMessage(text=event.message.text)
    )
```

如果我 `print` 出 event 的內容：
```python
for event in events:
    print("event text", event.message.text)
    if isinstance(event, MessageEvent):  # 如果有訊息事件
```
```shell
event text 哈囉
```
![](https://i.imgur.com/KbWxNDq.png)


## 綜合版

```python=
@csrf_exempt
def callback(request):
    if request.method == 'POST':
        signature = request.META['HTTP_X_LINE_SIGNATURE']
        body = request.body.decode('utf-8')

        try:
            events = parser.parse(body, signature)  # 傳入的事件
        except InvalidSignatureError:
            return HttpResponseForbidden()
        except LineBotApiError:
            return HttpResponseBadRequest()

        for event in events:
            print("event", event.message.id)
            # print("event", type(event))
            if isinstance(event, MessageEvent):  # 如果有訊息事件
                print("message", event.message)
                message = []
                if event.message.type == 'text':
                    text_ = event.message.text
                    message.append(TextSendMessage(text=text_))

                elif event.message.type == 'sticker':
                    text_ = event.message.id
                    message.append(TextSendMessage(text=text_))

                elif event.message.type == 'image':
                    text_ = event.message.id
                    message.append(TextSendMessage(text=text_))

                elif event.message.type == 'video':
                    text_ = event.message.id
                    message.append(TextSendMessage(text=text_))

                elif event.message.type == 'audio':
                    text_ = event.message.id
                    message.append(TextSendMessage(text=text_))

                elif event.message.type == 'location':
                    text_ = event.message.id
                    message.append(TextSendMessage(text=text_))

                # 回復傳入的訊息文字
                line_bot_api.reply_message( event.reply_token, message )

            
        return HttpResponse()
    else:
        return HttpResponseBadRequest()
```

我們這邊除了文字訊息以外，其他都回傳訊息 id 給使用者，我們來看看發生甚麼事。

![](https://i.imgur.com/28bXBa1.jpg)

我們先從這部分開始，之後會一個一個解釋。

## Message Type

**Message Type** 有以下幾種（昨天也講過了）
+ text
+ sticker
+ image
+ video
+ audio
+ location

每一個 `type` 的 `event` 裡的內容都不同。我們會在之後的天數慢慢把細節補齊（坑一定會補完的！）

這裡我們先講 `Text` 的部分
這些就是我們在 `Text` 這個 `event` 裡面有的所有內容。

```json
{
  "events": [
    {
      "replyToken": "nHuyWiB7yP5Zw52FIkcQobQuGDXCTA",
      "type": "message",
      "mode": "active",
      "timestamp": 1462629479859,
      "source": {
        "type": "user",
        "userId": "U4af4980629..."
      },
      "message": {
        "id": "325708",
        "type": "text",
        "text": "@example Hello, world! (love)",
        "emojis": [
          {
            "index": 23,
            "length": 6,
            "productId": "5ac1bfd5040ab15980c9b435",
            "emojiId": "001"
          }
        ],
        "mention": {
          "mentionees": [
            {
              "index": 0,
              "length": 8,
              "userId": "U850014438e..."
            }
          ]
        }
      }
    }
  ]
}
```

我們所傳入的訊息被儲存在 `event.message.text` 裡面，如果我們要使用的話，也可以隨時拿取裡面所需的內容。

一開始的 `echo`，就是指 ` TextSendMessage(text=event.message.text)`
我們回傳的是自己傳進來的文字！

---

之後會填上所有的坑，包含 Line Bot 的應用。
今天就先稍微講到這裡！