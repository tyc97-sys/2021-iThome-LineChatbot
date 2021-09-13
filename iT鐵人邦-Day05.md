---
title: iT鐵人邦-Day05
tags: iT鐵人邦
---

# 【Day 05】從零開始的 Line Chatbot－公開 APP 網址

昨天稍微簡介了一下 ngrok 在做甚麼，今天我們要讓我們的 Line Channel 公開，這個 Chatbot 才可以真正運作！

> ngrok 是一個可以讓內網伺服器與外界溝通的一個服務。

## 安裝

我們先到 [Ngrok 的官網](https://ngrok.com/) 依據作業系統進行下載即可。

![](https://i.imgur.com/tsEaXzR.png)

因為我的系統是 Windows，他自動幫我選擇了 Download for Windows（其他作業系統請自行選擇）。
![](https://i.imgur.com/JSaoYlu.png)

按照 [get-started setup](https://dashboard.ngrok.com/get-started/setup) 一步一步進行即可。

下載之後會是一個壓縮檔，直接解壓縮即可（會產生一個執行檔 `.exe`）。

![](https://i.imgur.com/oJKS5Ta.png)

之後你會在網站上看到一個 authtoken，要注意好這個 token！！
![](https://i.imgur.com/wkJXKXt.png)

之後我們執行剛剛解壓縮的執行檔
![](https://i.imgur.com/6aEfnw2.png)

輸入以下指令：

```shell
ngrok authtoken <YOUR TOKEN>
```

接著，就可以透過Ngrok，將本機的埠號對外公開。

### 透過 ngrok 公開網址

以我的 channel 為例，Django在本機運行的埠號為8000，所以輸入以下的指令：

```shell
ngrok http 8000
```
執行結果如下
![](https://i.imgur.com/h9tKR3d.png)

被**紅色框框**框起來的即為公開後的網址，而我們的 `localhost` 是 `https://localhost:8000`。

> 要特別注意的是，每次重新連接後的網址都不同，因為是隨機產生的一個對外公開的 https 網址，所以每一次重新連接，以下的東西都要重新更改一次。

我們把產生出來的一個 https 網址填入 Line Developers 的 `Messageing API > Webhook settings > Webhook URL`，不過還要再後面接上 Line Bot APP 的網址，如圖：

![](https://i.imgur.com/r3QaVt2.png)

然後我們也要填入專案主程式的 setting，`mylinebot/settings.py`：
```python
ALLOWED_HOSTS = [
    '428b1f34284f.ngrok.io'    #允許的網域名稱
]
```

### Line Developer Setting

好了，到這一步，我們已經快要可以執行了！

我們要做最後一步的設定。在 `Messaging API > LINE Official Account features > Auto-reply messages > Edit`

![](https://i.imgur.com/TzBunSQ.png)

我們可以看到一些設定的部分。
+ Main settings
    + Response mode：我們選擇 Bot 就可以讓我們撰寫的聊天機器人自動回復
    + Greeting message：可以選擇加入好友後是否要先傳一個招呼訊息（也可以編輯）
+ Detailed settings
    + Auto-response：要記得把 Auto-response 關掉
    + Webhooks：這個選項要記得打開(Enabled)。

都設定完後，Line Channel 就能夠與 Line Bot APP 互相連結

## 執行！

最後，利用以下指令就可以執行 Line Bot APP（回到專案資料夾，開啟 `Terminal`）

```shell
python manage.py runserver
```

即可執行！

**不過！！**

### 如果更新過 `NutritionBot > models.py`

請先執行
```shell
python manage.py makemigrations NutritionBot
python manage.py migrate
```

更新完 models.py 以後才可以執行

```shell
python manage.py runserver
```

---

那 model 是甚麼，我們後續會繼續講關於 django 框架的細節～。
那就下集待續啦！！