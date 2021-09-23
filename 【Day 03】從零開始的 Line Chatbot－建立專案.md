# 【Day 03】從零開始的 Line Chatbot－建立專案
###### tags: iT鐵人邦
今天要來正式建立 Chatbot 的 Python 專案！

## 登入資料

進入 [Line Developers](https://developers.line.biz/zh-hant/) 直接登入自己的 Line 帳號。

如果是第一次建立開發者帳號，需要先填寫一些基本資料（僅限第一次登入時才要）。
![](https://i.imgur.com/h0G4wv7.png)

之後，建立一個 `Provider`。
`Provider` 指的是提供服務的**個人**或**公司**。
![](https://i.imgur.com/XFlLIEm.png)


## 建立 Messaging API channel

接著我們就要建立一個 Messaging API 的 channel。

![](https://i.imgur.com/bqL33Ks.png)

繼續填寫資料~
![](https://i.imgur.com/odQQGRJ.png)

**基本設定**

| 項目 | 畫面 |
| :--: | :--: | 
| Channel icon（optional）| Chatbot 的大頭貼 |
| Channel name | Chatbot 名字 |
| Channel description | 啟動時的同意權限畫面（僅針對有建立 LIFF 的 Channel） |
| Privacy policy URL（optional） | - |
| Terms of use URL（optional） | - |
 
這之後我們就可以在主頁上看到我們建立好的 channel 了！！
![](https://i.imgur.com/VWEbmBD.png)

---

在 Line Developers 上該建立的東西到這邊基本上就幾乎完成了，但是**先不要把網頁關掉**！

在建立 Python 專案時，還需要回來這個網頁複製兩個憑證。（等我們要用到的時候再回來）

## Chatbot 專案

首先，我們要建置環境，安裝以下之後會使用到的 Python 套件

```
$ pip install django
$ pip install line-bot-sdk
$ pip install requests
```

接著建立 Django 專案

```
#建立Django專案
$ django-admin startproject mylinebot .

#建立Django應用程式 程式名稱 django_chatbot
$ python manage.py startapp django_chatbot

$ python manage.py migrate  
```

此時
- `mylinebot` 是**專案主程式**
- `django_chatbot` 是**應用程式**

### 新增憑證

我們可以回到 Line Developers，需要拿到以下兩個憑證。
![](https://i.imgur.com/SDZjIyi.png)

**Basic Settings 裡的 Channel Secret**
![](https://i.imgur.com/VQ45VXl.png)

**Messaging API 裡的 Channel access token**，是 long-lived 的
![](https://i.imgur.com/5uUD8Zl.png)


之後開啟專案，我們在 `mylinebot/settings.py` 裡增加上面提到的兩個憑證。

```python
LINE_CHANNEL_ACCESS_TOKEN = 'Messaging API > Channel access token'

LINE_CHANNEL_SECRET = 'Basic settings > Channel secret'
```
直接把兩個憑證複製上去（`type: str`）。

然後在同一個 `settings.py` 檔裡面的 `INSTALLED_APPS` 加上剛剛所建立的 Django 應用程式，如下：
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    'django_chatbot.apps.DjangoChatbotConfig',    #新增APP名稱
]
```

也就是在 `django_chatbot/apps.py` 裡的 `class`

```python
class DjangoChatbotConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'django_chatbot'
```

### 寫一個最最最基本的 `echo` Chatbot

我們先來看另一個檔案 `django_chatbot/views.py`，這個檔案可以算是 Chatbot 的主程式(?)。
我們會把 Chatbot 接收訊息後要執行的內容寫在 `def callback(request):` 裡。

我們來試寫一個最基本的 Chatbot，`echo`，Chatbot 會回傳使用者輸入的文字內容。
*範例取自官方 documentation*

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
            if isinstance(event, MessageEvent):  # 如果有訊息事件
                line_bot_api.reply_message(  # 回復傳入的訊息文字
                    event.reply_token,
                    TextSendMessage(text=event.message.text)
                )
        return HttpResponse()
    else:
        return HttpResponseBadRequest()
```

---

**雖然目前我們把 Chatbot 的主要功能都寫出來的，可是到目前為止，還不能執行。**

我們要把網址公開才可以讓這個機器人連結到我們的 channel，這樣機器人才可以回應我們寫的主要功能。
明天會繼續講解如何公開網址連結 channel。

*之後的幾天都會慢慢把挖出來的坑給填起來！*


Ref.

- [Line Developers Documentation > Getting started with the Messaging API](https://developers.line.biz/en/docs/messaging-api/getting-started/)
