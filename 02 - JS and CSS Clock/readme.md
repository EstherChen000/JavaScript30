# 02 - JS and CSS Clock

## :dart: 目標

1. 取得目前時分秒。
2. 使時鐘指針隨時分秒轉動。

## :bomb: 注意

- 設定旋轉中心點
  - `transform-origin: 100%` 或是 `transform-origin: right`

- 調整指針的卡頓效果`transition-timing-function` [參考](https://ithelp.ithome.com.tw/articles/10195313)

- 指針起始位置
  - (CSS)畫面調整歸零
  - `transform: rotate(90deg);`
  - (JS)目前時間角度 + 轉動角度(以秒針為例)
  - `const secondsDegrees = ((seconds / 60) * 360) + 90;`

- 善用Chrome的開發者工具快速調整效果
  - `rotate()`旋轉角度調整
  - `cubic-bezier`貝茲曲線調整

- 問題：當指針從 444deg -> 90deg 的過程中，畫面也會同步呈現角度的變化，造成指針一瞬間的飄移。有兩種解決方法：
  - 方法1 取得一次時間度數後，持續累加角度

    ```javascript
      const secondHand = document.querySelector('.second-hand');
      const minHand = document.querySelector('.min-hand');
      const hourHand = document.querySelector('.hour-hand');
      let secondDeg = 0,
        minDeg = 0,
        hourDeg = 0;

      function setDate() {
        const now = new Date();
        const seconds = now.getSeconds();
        secondDeg = (seconds / 60) * 360 + 90;

        const mins = now.getMinutes();
        minDeg = (mins / 60) * 360 + 90;

        const hours = now.getHours();
        hourDeg = (hours / 12) * 360 + 90;
      }

      function updateDate() {
        secondDeg += (1 / 60) * 360;
        minDeg += (1 / 60 / 60) * 360;
        hourDeg += (1 / 60 / 60 / 12) * 360;
        secondHand.style.transform = `rotate(${secondDeg}deg)`;
        minHand.style.transform = `rotate(${minDeg}deg)`;
        hourHand.style.transform = `rotate(${hourDeg}deg)`;
      }

      setDate();
      setInterval(updateDate, 1000);
    ```

  - 方法2 當指針回到 90deg 時，取消動畫效果(持續時間)

    ```javascript
      const secondHand = document.querySelector('.second-hand');
      const minHand = document.querySelector('.min-hand');
      const hourHand = document.querySelector('.hour-hand');
      function setDate() {
        const now = new Date();
        const seconds = now.getSeconds();
        const secondsDegrees = (seconds / 60) * 360 + 90;
        secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

        const mins = now.getMinutes();
        const minsDegrees = (mins / 60) * 360 + 90;
        minHand.style.transform = `rotate(${minsDegrees}deg)`;

        const hours = now.getHours();
        const hoursDegrees = (hours / 12) * 360 + 90;
        hourHand.style.transform = `rotate(${hoursDegrees}deg)`;

        if (secondsDegrees === 90) {
          secondHand.style.transition = 'all 0s';
        } else {
          secondHand.style.transition = 'all 0.05s';
        }

        if (minsDegrees === 90) {
          minHand.style.transition = 'all 0s';
        } else {
          minHand.style.transition = 'all 0.05s';
        }
      }

      setInterval(setDate, 1000);
    ```

## :old_key: 語法

### 時間取得

`new Date()` 以當前日期時間創建一個新的日期物件

`getSeconds()` 從日期物件中獲得秒數(0-59)

`getMinutes()` 從日期物件中獲得分鐘數(0-59)

`getHours()` 從日期物件中獲得小時數(0-23)

如下方使用，[完整方法參考](https://www.w3schools.com/jsref/jsref_obj_date.asp)

```javascript
const d = new Date();
d.getHours();
```

### 定時器

`setInterval(func, delay)` 以指定的週期，**持續**呼叫函式。delay的單位為毫秒(1000毫秒 = 1秒)

`setTimeout(func, delay)` 再指定的時間後，呼叫函式**一次**。delay的單位為毫秒(1000毫秒 = 1秒)

> 另外，當使用定時器時，若呼叫的函式中有`this`的情況下須特別注意，此時`this`會指向全域物件(window)，[參考](https://developer.mozilla.org/zh-CN/docs/Web/API/setInterval#%E2%80%9Cthis%E2%80%9D%E7%9A%84%E9%97%AE%E9%A2%98)

## :beer: DEMO
