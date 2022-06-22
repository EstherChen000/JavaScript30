# 09 - Dev Tools Domination

## :dart: 目標

- 學會14個開發技巧
  - console 技巧 13 個
  - chrome 開發者工具技巧 1 個

## :bomb: 注意

- 設置斷點：為程式碼設定運行時的中斷處，中斷後可利用各種Debugger工具，逐行檢查程式碼或查看當下作用域的變數等等。
  - DOM 更改斷點：
    1. 打開開發者工具的element分頁
    2. 右鍵點擊要設置斷點的元素
    3. 滑鼠移至Break on...上方
    - subtree modifications: 當目前選擇的節點的子節點，被移除或添加、或是子節點的內容被更改時除發。子節點的屬性變更或是目前節點的任何更改，皆不會觸發。
    - attribute modifications: 當目前選擇的節點上，添加或刪除屬性、或是屬性值更改時觸發。
    - Node removal: 當目前選擇的節點被移除時觸發。

## :old_key: 語法

### console

#### 【#1】`console.log()` + 常規寫法

- 常規寫法

  ```javascript
  console.log('Hello World!');//Hello World!
  ```

#### 【#2】`console.log()` + 插值寫法

- 控制字元

  ```javascript
  console.log('This is my %s', '🎯');//This is my 🎯
  ```

  > %s 取代字串

- 樣板字符串

  ```javascript
  const name = 'Esther';
  console.log(`My name is ${name} !`);//My name is Esther !
  ```

#### 【#3】`console.log()` + 增加樣式

- 控制字元

  ```javascript
  console.log('%c OH!', 'font-size:50px; color:yellow;');
  ```

  > %c 取代CSS格式的字串
  >
  > 在C語言中，%c 取代字元(chat)

#### 【#4】`console.warn()` 輸出警告訊息

```javascript
console.warn('這是一個警告!');
```

#### 【#5】`console.error();` 輸出錯誤訊息

```javascript
console.error('這是一個錯誤!');
```

#### 【#6】`console.info();` 輸出訊息

```javascript
console.info('這是一個信息');
```

#### 【#7】`console.assert();` 測試

如果斷言（assertion）為非（false），主控台會顯示錯誤訊息；如果斷言為是（true），則不發生任何事。

```javascript
const p = document.querySelector('p');
console.assert(p.classList.contains('btn'), '缺少btn!');
```

#### 【#8】`console.clear();` 清空console

```javascript
console.clear();
```

#### 【#9】`console.dir();` 列出DOM元素

讓主控台列出物件屬性

```javascript
console.dir();
```

#### 【#10】`console.groupCollapsed();`與`console.End();` 建立群組

用`console.groupCollapsed();`使主控台的輸出成為一個可折疊的群組，再以`console.End();`讓群組結束。

```javascript
dogs.forEach((dog) => {
  console.groupCollapsed(`${dog.name}`);
  console.log(`This is ${dog.name}`);
  console.log(`${dog.name} is ${dog.age} years old`);
  console.log(`${dog.name} is ${dog.age * 7} dog years old`);
  console.groupEnd(`${dog.name}`);
});
```

> 也可以用`group()`代替`groupCollapsed()`做開頭，前者在輸出中呈現展開的折疊頁

#### 【#11】`console.count();` 計數參數出現的次數

```javascript
console.count('Ana');//Ana: 1
console.count('Ana');//Ana: 2
console.count('Jane');//Jane: 1
```

#### 【#12】`console.time();`和`console.timeEnd()` 計算花費時間

每個計時器必須擁有唯一的名字

```javascript
console.time('fetching data');
fetch('https://api.github.com/users/wesbos')
  .then((data) => data.json())
  .then((data) => {
    console.timeEnd('fetching data');//fetching data: 538.471923828125 ms
  });
```

#### 【#13】`console.table();` 將資料以表格方式呈現

```javascript
console.table(dogs);
```

## :beer: DEMO
