# 【Day 09】Text Message 應用
###### tags: iT鐵人邦
前一天我們講到綜合版可以接收所有種類的訊息。

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

那我們先專心講解關於 `type()` 為 `text` 的訊息

## `tyep == text`
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
                line_bot_api.reply_message( event.reply_token, message )
                
        return HttpResponse()
    else:
        return HttpResponseBadRequest()
```

我們在 `line 19` 宣告了一個 list，`message` 好讓我們之後 `line_bot_api.reply_message()` 推送訊息比較方便。

這邊跟之前一樣都沒有改過，就是回推送傳過的訊息。

那我們改成如果遇到關鍵字就傳送某些訊息的話...

```python=
from django.shortcuts import render

# Create your views here.

from django.shortcuts import render
from django.http import HttpResponse, HttpResponseBadRequest, HttpResponseForbidden
from django.views.decorators.csrf import csrf_exempt
from django.conf import settings

from linebot import LineBotApi, WebhookParser
from linebot.exceptions import InvalidSignatureError, LineBotApiError
from linebot.models import MessageEvent, TextSendMessage
import os

line_bot_api = LineBotApi(settings.LINE_CHANNEL_ACCESS_TOKEN)
parser = WebhookParser(settings.LINE_CHANNEL_SECRET)

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
                    mtext = event.message.text
                    if "嗨" in mtext:
                        text_ = "哈囉你好"
                    else:
                        text_ = 'What?'

                    message.append(TextSendMessage(text=text_))


                # 回復傳入的訊息文字
                line_bot_api.reply_message( event.reply_token, message )

        return HttpResponse()
    else:
        return HttpResponseBadRequest()
```
```python=38
if event.message.type == 'text':
    mtext = event.message.text
    if "嗨" in mtext:
        text_ = "哈囉你好"
    else:
        text_ = 'What?'
```

![](https://i.imgur.com/irXsUSq.png)

我們看一下 mtext 裡面有甚麼：
```shell
mtext: 嗨
```
```shell
mtext: 嗚嗚嗚
```
```shell
type of mtext: <class 'str'>
```

這裡我們可以看到 `mtext` 是 `str` 型態
所以這裡就可以透過邏輯運算來處理文字訊息。

當然，這裡其實都算是透過官版就可以處理的問題，我們明天就來看看基本的按鈕選項(Button Template)


---

### 補充

補充一個在 ngrok 連線時**有可能**發生的問題：

啟動 ngrok 連線卻出現以下錯誤：

```shell
Your account '' is limited to 1 similtaneous ngrok client session.
Active ngrok client sessions in region 'us':
    ......

ERR_NGROK_108
```
![](https://i.imgur.com/TcdDfqH.png)

這段錯誤基本上是在說，帳戶只允許一個 client session。
附上[官網傳送門](https://ngrok.com/docs#getting-started-expose)


這邊自己的解決方式就只是換區而以：
```shell
ngrok http -region ap 8000
```

供大家參考參考

<!--
最基本

確認

-->
