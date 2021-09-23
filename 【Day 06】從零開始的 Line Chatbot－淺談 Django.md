# 【Day 06】從零開始的 Line Chatbot－淺談 Django
###### tags: iT鐵人邦
前幾天在做建立專案的時候，好像看到一個不是很懂的東西－**Django**。
今天不談 Chatbot，我們來淺談一下 Django 是甚麼東西。

## Django

> Django 是一個開放原始碼的 *Web 應用框架*，由 Python 寫成。採用了 MVT 的軟體設計模式，即模型（**M**odel），視圖（**V**iew）和模板（**T**emplate）。

[*Django wiki*](https://zh.wikipedia.org/wiki/Django)

### What is Django

Django 念法：$\text{/jang-goh}$，是一個用 Python 並且免費開放原始碼的 Web 應用程式框架。
有點像是用一堆模組，去快速組裝開發網站（我覺得可以把它想像成樂高積木）

### Why do you need a framework?

要瞭解 Django 之前，我們要先瞭解伺服器（server）的運作。
也就是伺服器需要知道我們想要用怎樣的方式來提供網頁。

大家在**網路的領域裡**多多少少應該都會聽過埠（Port）還有請求（Request）。

我們把埠（Port）想像成**信箱**，會偵測寄來的**信件**，也就是請求（Request）。
Web 伺服器會讀這些信（請求），然後網頁就會做出相對應的回應。

Django 就是做這些工作！（幫我們產生相對應的回應）

### What happens when someone makes a request to your server?

Server 收到請求，之後會透過 Django 判斷這個請求是甚麼，然後給出相對應的回應。
這部分是由 Django 的 urlresolver 來處理。
> URL - Uniform Resource Locator，其實就是所謂的網址。

如果 URL 符合某一個範例，Django 就送出這個請求相對應的函數們（在這裡稱為 `view`）

>再用信件去比喻：
郵差會去確認每家的地址把信送給他們。
如果地址對了就把信放進去。

這就是 urlresolver 在做的事情。

在 `view` funciton 中會做一些有趣的事情：
我們會去資料庫找資料。如果使用者如果去要求更改某些資料，`view` 會檢查你是不是允許他可以做這件事，然後你會在更新了他的工作敘述以後回傳給他一個「完成囉！」的訊息。之後 view 就會產生一個回應，Django 就會將回應送到使用者的瀏覽器上。

當然，這只是淺談，我們也不需要去理解更多的原理，大概知道這一些即可。

---

**Ref.**

+ [Django wiki](https://zh.wikipedia.org/wiki/Django)
+ [Web應用框架 wiki](https://zh.wikipedia.org/wiki/Web%E5%BA%94%E7%94%A8%E6%A1%86%E6%9E%B6)
+ [Django Girls' Tutorial 中文版教材 Django Girls 教學](https://carolhsu.gitbooks.io/django-girls-tutorial-traditional-chiness/content/index.html)