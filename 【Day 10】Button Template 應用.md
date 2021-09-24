# 【Day 10】Button Template 應用
###### tags: `iT鐵人邦`

今天我們來講怎麼使用 Line Messaging API 的 Button Template

先送上官網傳送門 [Template Message](https://developers.line.biz/en/docs/messaging-api/message-types/#template-messages)。

## Template Message 種類

1. Buttons，一般的按鈕
2. Confirm，確認按鈕
3. Carousel，可以橫向滑動的多選項按鈕
4. Image Carousel，多影像按鈕。

我們今天先來看看前兩種。

### Buttons Template Message

![](https://i.imgur.com/XZY6XF1.png)

![](https://i.imgur.com/IIGChqm.png)

我們做一個 Button，有兩個選項：
1. 是，第一次見面：點選後會回傳`是`。
2. 已經見過了：點選後會回傳`見過了`。

先直上程式碼！

```python=
from django.shortcuts import render

from django.shortcuts import render
from django.http import HttpResponse, HttpResponseBadRequest, HttpResponseForbidden
from django.views.decorators.csrf import csrf_exempt
from django.conf import settings

from linebot import LineBotApi, WebhookParser
from linebot.exceptions import InvalidSignatureError, LineBotApiError
from linebot.models import *

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
                        message.append(
                            TemplateSendMessage(
                                alt_text='Buttons template',
                                template=ButtonsTemplate(
                                    title='Hello',
                                    text='第一次見面嗎',
                                    actions=[
                                        MessageTemplateAction(
                                            label='是，第一次見面',
                                            text='是',
                                        ),
                                        MessageTemplateAction(
                                            label='已經見過了',
                                            text='見過了',
                                        ),
                                    ]
                                )
                            )
                        )



                    print("type of mtext: {}".format(type(mtext)))

                # 回復傳入的訊息文字
                line_bot_api.reply_message( event.reply_token, message )

        return HttpResponse()
    else:
        return HttpResponseBadRequest()

```

接著我們專心在這塊

```python=37
if "嗨" in mtext:
    message.append(
        TemplateSendMessage(
            alt_text='Buttons template',
                template=ButtonsTemplate(
                    title='Hello',
                    text='第一次見面嗎',
                    actions=[
                        MessageTemplateAction(
                            label='是，第一次見面',
                            text='是',
                        ),
                        MessageTemplateAction(
                            label='已經見過了',
                            text='見過了',
                        ),
                    ]
                )
        )
    )
```

我們順便來看看 Buttons template 的 json example
這個官方提供的範例顯示了所有可以使用的屬性（attribute）。
```json=
{
  "type": "template",
  "altText": "This is a buttons template",
  "template": {
      "type": "buttons",
      "thumbnailImageUrl": "https://example.com/bot/images/image.jpg",
      "imageAspectRatio": "rectangle",
      "imageSize": "cover",
      "imageBackgroundColor": "#FFFFFF",
      "title": "Menu",
      "text": "Please select",
      "defaultAction": {
          "type": "uri",
          "label": "View detail",
          "uri": "http://example.com/page/123"
      },
      "actions": [
          {
            "type": "postback",
            "label": "Buy",
            "data": "action=buy&itemid=123"
          },
          {
            "type": "postback",
            "label": "Add to cart",
            "data": "action=add&itemid=123"
          },
          {
            "type": "uri",
            "label": "View detail",
            "uri": "http://example.com/page/123"
          }
      ]
  }
}
```

我們那出想要（或是需要）用到的屬性，轉成 python 的寫法。
那我們就直接介紹比較常見的屬性。

+ `alt_text`：代表電腦如果無法顯示時，會在電腦上顯示的文字
+ `template`：這裡對應到 `json` 裡的
```json=
"template": {
      "type": "buttons",
```

剩下的 `title`、`text`、`label` 我們直接用下面這張圖片來解釋。
應該很清楚！
![](https://i.imgur.com/0cU9A7k.png)

+ `actions`：就是包含主體 **按鈕**，還有按下去的要做的動作。有多種選項可以選擇。
    + Postback action
    + Message action
    + URI action
    * Datetime picker action
    * Camera action
    * Camera roll action
    * Location action
    * Richmenu Switch Action 


不過我主要都使用 `Message action` 而以，所以我們目前也只針對這個。

```python
MessageTemplateAction(
    label='是，第一次見面',
    text='是',
),
```
`text`，表示當你點選按鈕後**送出**之後會傳送出去的字。


**但是要注意！**
按鈕最多只能放四個，就是 actions 裡面只能有四個。

