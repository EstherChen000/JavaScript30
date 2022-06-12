# 01 - JavaScript Drum Kit

## :dart: 目標

1. 當敲擊對應鍵盤時，播放指定樂器聲音。
2. 鍵盤對應螢幕上的字母也會有動畫效果產生。

## :bomb: 注意

- 善用HTML `data-*` Attribute
  - 搭配屬性選擇器與樣板字符串

- 重複敲擊鍵盤時，效果也應能重複的產生
  - 先停止撥放上一次敲擊鍵盤所產生的音效
  - `audio.currenTime=0`

- 如何將一個事件監聽器附加到多個元素?
  - 循環

    使用`forEach()`對它們進行迭代

    ```javascript
    function removeTransition(e) {
      if (e.propertyName !== 'transform') return;
      this.classList.remove('playing');
    }

    const keys = document.querySelectorAll('.key');
      keys.forEach((k) =>
        k.addEventListener('transitionend', removeTransition);
    );
    ```

  - 事件委派(Event delegation)

    將事件監聽器綁在上層的元素

    ```javascript
    function removeTransition(e) {
      if (e.propertyName !== 'transform') return;
      e.target.classList.remove('playing');
    }

    const keys = document.querySelector('.keys');
    keys.addEventListener('transitionend', removeTransition);
    ```

## :old_key: 語法

### `element .addEventListener( event , function , useCapture )`

event： 必須。字串，事件名，不要使用'on'的前綴。[事件參考](http://www.w3big.com/zh-TW/jsref/dom-obj-event.html)

function： 必須。事件觸發時執行的函式。事件物件會作為第一個參數傳入函式，例如：以 `window.addEventListener('keydown', playSound);`來說：

```javascript
function playSound(e){
  console.log(e);
  //KeyboardEvent {isTrusted: true, key: 's', code: 'KeyS', location: 0, ctrlKey: false, …}
  //本次大量出現的keyCode屬性出自於此
}
```

useCapture：可選。布林值，默認 false ，於冒泡(bubbling phase)階段執行。 [冒泡與捕獲階段參考](https://zh.javascript.info/bubbling-and-capturing)

### querySelector() 與 querySelectorAll()

querySelector(selectors)： 回傳第一個符合 selectors 的**元素**其值。

querySelectorAll(selectors)： 回傳一個靜態 NodeList物件(類似陣列)。NodeList物件不推薦使用for...in來遍歷，它可使用幾種方法如：`forEach()`， `entries()`， `item()`， `keys()`， `values()`。如果想使用其他陣列方法須將NodeList物件轉成陣列：`Array.from()`

selectors：CSS選擇器，須以''包覆

## 元素的class屬性如何修改

[classList 方法列表](https://www.w3schools.com/jsref/prop_element_classlist.asp)
幾種常用如下：

```javascript
const div = document.createElement('div');

//新增class
div.classList.add('visible');

//移除class
div.classList.remove('visible');

//新增/移除class
div.classList.toggle('visible');
```

## :beer: DEMO
