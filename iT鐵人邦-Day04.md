# 【Day 04】從零開始的 Line Chatbot－連線

昨天提到建立 Chatbot 專案，但是雖然已經把主要的邏輯運算都寫好了，但是還不能執行，因為我們還沒有進行連線！

## 連線

在 `def callback` 中，會偵測到使用者是否有事件傳入（也就是有沒有使用者傳入訊息），之後就會透過 Python 迴圈去處理。

不過，接下來我們還要設定 Line Bot APP 的連結網址，所以我們要建立一個**新的檔案** `Nutrition/urls.py`，加入以下網址設定：

```python
from django.urls import path
from . import views
 
urlpatterns = [
    path('callback', views.callback)
]
```

之後我們要將這個 APP 網址加到專案主程式中，所以在 `mylinebot/urls.py` 檔案中加入下面網址設定：

```python
from django.contrib import admin
from django.urls import path, include
 
urlpatterns = [
    path('admin/', admin.site.urls),
    path('django_chatbot/', include('django_chatbot.urls')) # 包含應用程式的網址
]
```

好了！現在的專案差不多準備好了！

要怎麼讓這個 Channel 公開呢？

## Ngrok

![](https://i.imgur.com/fDkTbw8.png)

我們今天先來介紹什麼是 Ngrok。

[ngrok Documentation](https://ngrok.com/docs)

Ngrok 是甚麼呢？他是一個**反向代理**。
簡單來說，就是可以輕鬆地讓我們在本地端建立的內網 `localhost` 與外網伺服器去進行溝通。
那，外網又是甚麼？外網就是可以讓我們擁有一個公開的網址，讓其他人連上該網址的內容。

我們開發 project 都會在 local 端進行，如果後端開發好 API 想要給前端的話要怎麼辦？當然，我們可以將我們的 API 架在一台伺服器上，但是對新手來說太複雜了，所以現在有所謂架設雲端伺服器的方式來放置我們的 API，其中一個好用的平台－Heroku。

不過我們現在撰寫 Chatbot 不需要架設一個雲端伺服器，我們只需要當下快速測試的話，使用 **ngrok** 即可！

Ngrok 的背景原理就是將你的本機指定的地址連接到 ngrok 的雲端伺服器，在轉換成一串公開網址來存取你本機的內容。

> Ngrok 優點：
> 1. 快速
> 2. 提供 https 服務讓使用上更安全
> 3. 甚至可以設置密碼保護

![](https://i.imgur.com/MVyO3Q4.png)


---

那麼 ngrok 簡單的介紹就先到這裡，我們明天下集待續...

Ref.

+ [善用 ngrok 服務讓佈署本機服務更 Easy！](https://aidaidme.com/ngrok-deploy-to-network/)
+ [快速讓外網連接本機的利器 — ngrok](https://medium.com/%E4%BC%81%E9%B5%9D%E4%B9%9F%E6%87%82%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88/%E5%BF%AB%E9%80%9F%E8%AE%93%E5%A4%96%E7%B6%B2%E9%80%A3%E6%8E%A5%E6%9C%AC%E6%A9%9F%E7%9A%84%E5%88%A9%E5%99%A8-ngrok-ac92f792e1f0)
