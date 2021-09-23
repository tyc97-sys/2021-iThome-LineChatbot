# 【Day 02】從零開始的 Line Chatbot 系統－序章 Part 2
###### tags: iT鐵人邦
## 認識一些軟體開發的專業術語

在做軟體專案的時候，常常會看到一些英文簡寫，像是 Day 01 講到的 API，還有像是 IDE、SDK 等等術語。

那我們就接著昨天講過的 API，再稍微介紹一下這些基本術語。

## IDE

IDE，全稱 Integrated Development Environment，或是 Integration Design Environment 也可以稱為 Integration Debugging Environment。

嗯...英文，看起來都差不多，簡單來講，我們可以把 IDE 翻譯成**整合開發環境**。

那...

### IDE 是要做甚麼的？

IDE 可以輕鬆的讓工程師開發出一個專案，因為他可以整合在 coding 的時候需要的所有工具，透過這些工具可以大大的提高工程師的效率。

並且這些在 coding 的時候所需要的常用工具都會呈現在一個視窗內，也就是通常 IDE 都會是一個 GUI（Graphical User Interface，圖形使用者介面）

### Editor

開發一個專案最重要的地方當然是**寫**程式，所以我們需要一個編輯器（Editor），讓我們從一個空白文件開始撰寫 code。

而 IDE 可以根據對應的語言去**高亮（Highlighting）**對應的語法。

例如：

+ 如果我們使用一般文字編輯器
```
with open(path, 'r', encoding='utf-8') as f:
    output = json.load(f)
```

+ 如果有 IDE
```python
with open(path, 'r', encoding='utf-8') as f:
    output = json.load(f)
```

這樣的功能會使程式的可讀性大大增加，效率一樣大大增加！

### Autocomplete

如果 IDE 知道你正在撰寫甚麼語言，他可以預測你接下來可能要寫的內容。（強大的 IDE 會有更強大的預測功能）

![](https://i.imgur.com/7yZ47UT.png)
就像是我用了 Python 的 IDE，輸入了 `impo`，就會出現以下的選項，這就是**預測功能**

### Compiler
編譯語言需要一個編譯器（Compiler）。
以 C語言為例，我們需要先將程式碼編譯成 object code，再透過 linker 產生執行檔 .exe。

而 IDE 可以一鍵進行 **Compiler** 跟 **Run**。

### Debugging

每個工程師不免在開發專案的時候會遇到最討人厭的 bug，而 IDE 也會提供好用的偵錯器（debugger），也可以在 compile 後，execute 前先找到錯誤的地方，中斷執行。

### 小結
以上就是 IDE 的好處。

有些強大的 IDE 可以支援多種不同的語言，有些 IDE 僅針對特定語言。

找到好的 IDE，還有適合自己的 IDE 可以大大增加 coding 效率，這就是所謂 **工欲善其事，必先利其器**！


## SDK

下一個在軟體開發常見的術語－SDK，Software Development Kit，中文翻譯：軟體開發套件。

SDK 被開發出來是為了減少工程師的工作量。

SDK 會包含整個軟體在特定平台上開發的工具，會是一整個大包集合，同時 API 也有可能會被一起打包進 SDK 裡面，同時 SDK 可以打包超過一個以上的 APIs。

假設你要開發一個 Windows 系統上的功能，我們就可以使用 Windows SDK，要開發 iOS 系統上的功能，我們可以使用 iOS SDK。使用這些 SDK 就可以在開始寫程式前弄好該用的環境設定，這可以大大減少工程師的工作量。

### API vs SDK

他們兩者的差異就在於，SDK 是一個集大成的**工具包**（或可以稱為工具箱，或是...工具人XD）

上面講到，如果我們要開發 iOS 環境可以先使用 iOS SDK，裡面就會包含多種 APIs，可單獨使用或是互相呼叫，並且還會有 documentation（官方手冊，我都這麼稱呼它）供開發者參考，最後可能還有模擬器可以模擬在 iOS 環境上的

所以什麼時候會單獨使用到 API？
當你是在**網路**開發一個功能，例如 Google Map API，我想在我的網頁設計裡加上 Google Map 的功能我就會去調用他們開發的 API 直接使用。

---

Day 02 字數有點多，才能讓我們盡快進入 Line Chatbot 的主題！



**Ref.**
- [What Is an IDE?](https://www.codecademy.com/articles/what-is-an-ide)