# Chapter 3 函式

## 簡短

- 函式不應該太長，一個函式建議不要大於 20 行，每個都要透露出本身的意圖，並要能引領到下個函式。
- 條件敘述都應該只有一行，且那行程式通常是函式呼叫敘述。這也代表函式不應該大到包含巢狀結構。

## 只做一件事情

一個函式只應該做一件事。若一個函式可以被提煉出兩個以上的抽象概念、或是可以被拆解成兩個以上的段落，那就是做了超過一件事情，可以將其拆分成不同的函式。

## 每個函式只有一層抽象概念
一個函式若含有混合層次的抽象概念，會讓人無法分辨某個表達式是一個基本概念還是一個細節。

我們希望程式碼的每個函式後面都緊接著下一層次的抽象概念，這個方法原則稱為**降層準則**，其實作成果為函式中的每個段落都敘述著目前所處的抽象層次，並且提及了接續的下個層次的函式。

## Switch 敘述
從 switch 敘述的本質來看，它永遠都在做多件事情，而且我們很難避免使用它，但是我們可以選擇將每個 switch 敘述都藏在較低抽象層次的函式或類別裡。

舉例來說，一個 switch 敘述對傳入的類別分別做不同的處理，那我們可以將其改成多型（Polymorphism）來達到目的。

## 使用具描述能力的名稱
不用害怕去取一個較長的函式名稱，一個較長但具描述性質的名稱，比一個較短但難以理解的名稱、或是一個較長且具描述性質的註解來得好。

## 參數
函式的參數數量，最理想是零個，再不然就是一個或兩個，盡量避免使用三個以上的參數。原因有二：
1. 參數具有透露概念的能力，其使用會強迫閱讀者了解可能並不那麼重要的細節。
2. 從測試的角度看，在參數數量多時，要考慮到所有參數可能的組合會較為困難。

### 單一參數的函式
傳遞單一參數至函數中有幾個常見的理由：
1. 詢問和這個參數有關的問題
2. 對這個參數進行某種操作並回傳
3. 利用參數去呼叫特定事件

如果不符合以上三種形式，試著不要使用單一參數。

### 旗標（flag）參數
使用旗標參數是不好的做法，等於宣告了一個函式做了不只一件事。最好的做法應該是將一個函式拆成兩個，一個在值為 true 時使用，一個在 false 時使用。

### 兩個參數的函式
在使用包含兩個參數的函式時，除了需要花時間理解參數的意義之外，還需要注意參數排列的順序，因此應該要盡量把兩個參數轉換成一個參數，像是將參數包成一個物件或類別傳入。

### 動詞和關鍵字
除了使用能表意的動詞之外，在函式名稱中加入能和參數連結的關鍵字也是很好的選擇，像是將 `assertEquals(expected, actual)` 改成 `assertExpectedEqualsActual(expected, actual)`，就減輕了需要記住參數順序的負擔。

## 要無副作用
當函式在其原本的工作下，又做了其他的事，可能就會產生副作用，導致時空耦合或是順序相依的問題。原則上函式應該只做一件事，但當不得已時，也應該反映在函式的名稱上。

## 指令和查詢的分離
函式應該要**能做某件事**或是**能回答某個問題**，但是兩者不該同時發生，
- **指令（command）型**的函式用來進行某個動作，像是 `set()` 或 `append()`，沒有回傳值
- **查詢（query）型**的函式用來回答問題，像是 `isAttributeExists()`，會回傳想得到的資訊

## 使用例外處理取代回傳錯誤碼
要指令型的函式回傳錯誤碼除了違背「指令和查詢分離」的原則之外，還可能造成以下問題：
1. 導致深層的巢狀結構，像是
  ```js
  if (deletePage(page) === OK) {
    if (deleteReference(page.name) === OK) {
      ...
    }
  }
  ```
2. 錯誤碼的類型可能是由一個 enum 定義，這樣當 enum 有所改變時，就會需要查看以往有用到錯誤碼的地方並作相容或修改，這也是一個開放封閉原則（OCP）的範例

較好的做法是使用**例外處理**，那錯誤處理的程式碼就可以從主要路徑中抽離出來。

## 不要重複自己（DRY 原則）
就像 OOP 就是將程式碼集中到基本類別裡，函式的設計也要盡量往不重複自己前進。