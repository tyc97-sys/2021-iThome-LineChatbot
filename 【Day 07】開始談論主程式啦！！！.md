# 【Day 07】開始談論主程式啦！！！
###### tags: iT鐵人邦
某一天我們提到，主要的邏輯都寫在 `django_chatbot/views.py`。
但是裡面牽涉太多種 function 了。

在經過幾天的思考，打算一個一個功能講（跟網路上其他的教學方式比較不太一樣），所以看完三十天，應該就能夠自己建立一個功能齊全的 Line Chatbot 囉！

## `def callback(request)`

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
```

這段程式碼的意思就是指
> 如果有`事件`傳入，就會進行到 `callback` 函式裡面的程式碼。

中間的 `try...except` 部分我們不詳談，因為那是固定的模式！

## `event`

```python=
# 當有事件傳入
for event in events:
    if isinstance(event, MessageEvent): 
```
`事件`，指的是用戶傳送**任何資料**進來，就會開始測試迴圈內容。

那 **任何資料** 有哪些？


### Message Objects

包含
+ Text Message：一般的文字訊息。雖然還是有一些限制，但基本上就只是文字訊息而已。
+ Sticker message：貼圖訊息
+ Image message：照片訊息
+ Video message：影片訊息（雖然這個功能目前還不成熟）
+ Audio message：語音新聞
+ Location message：傳送地點
+ Imagemap message：傳送可點擊的影片訊息（可以點擊圖片跳轉至其他網頁）
+ Template messages：一些按鈕選項
+ Flex Message：更多客製化的訊息

之後會一一介紹每一個功能

---

回到 `def callback()`

```python=
if isinstance(event, MessageEvent):  # 如果有訊息事件
    line_bot_api.reply_message(  # 回復傳入的訊息文字
        event.reply_token,
        TextSendMessage(text=event.message.text)
    )
```

如果我把這個 event 印出來

```shell
{"message": {"id": "14765943962853", "text": "\u958b\u6703", "type": "text"}, "mode": "active", "replyToken": "e6c71c582b834585833f07d909e4fe4c", "source": {"type": "user", "userId": "Uaabbbdd6ba5a8630412b106b23def4d2"}, "timestamp": 1631932933913, "type": "message"}
[18/Sep/2021 02:42:15] "POST /django_chatbot/callback HTTP/1.1" 200 0
```

這就是一個 message 的 event 儲存下來的內容。

我們回傳訊息就需要靠 `line_bot_api.reply_message` 還有 `event` 裡的 `reply_token`。

`TextSendMessage()`，函式就是要**回傳** `text=` 的文字。
這裡回傳使用者傳入的訊息。

----

剩下的我們就在往後的日子慢慢講囉～