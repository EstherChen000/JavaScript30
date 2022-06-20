# 07 - Array Cardio Day 2

## :dart: 目標

熟悉陣列方法：`some()`、`every()`、`find()`、`findIndex()`。

## :bomb: 注意

- 移除陣列裡的元素（搭配`findIndex()`）
  - 使用`splice()`，**移除原本的陣列元素**。
  - 建立新陣列，挑選需要的元素，使用`slice()`，原本的陣列元素不會被改變。
    > `splice()`、`slice()`和`split()`在長相(?)和用途上容易混淆，簡單區別如下：
    >
    > `splice(開始改動的元素索引, 欲刪除的元素數目, 欲增加的元素)`：陣列方法，對原陣列進行增/刪，會改變原陣列。
    >
    > `slice(開始複製的索引, 停止複製的索引)`：陣列方法，對原陣列進行複製，不會改變原陣列。開始索引值的元素會複製，結束索引值的元素不會複製，舉例：slice(0,3)會複製陣列的第 0、1、2 索引的元素。
    >
    > `split(分割字符)`：字串方法，將字串分割並轉為陣列。

## :old_key: 語法

### `some()`

會檢查是否有陣列元素通過測試，有符合的元素就回傳`true`(並停止)，沒有元素通過就回傳`false`，使用空陣列測試也會回傳`false`。`some()`方法不會更改原始陣列。

```javascript
//使用回呼函式
function isBigger(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBigger);  // false
[12, 5, 8, 1, 4].some(isBigger); // true

//或是使用箭頭函式
//當程式碼只有單行時，可以省略return關鍵字和花括弧
[2, 5, 8, 1, 4].some(x => x > 10); //false
[12, 5, 8, 1, 4].some(x => x > 10); // true
```

### `every()`

會檢查是否**所有**陣列元素都通過測試，所有元素通過就回傳`true`，只要有任何一個元素沒有通過就回傳`false`，注意，**使用空陣列測試是回傳`true`**。`every()`方法不會更改原始陣列。

```javascript
//使用回呼函式
function isBigger(element, index, array) {
  return element > 10;
}

[12, 5, 18, 11, 41].every(isBigger);  // false
[12, 15, 18, 11, 41].every(isBigger); // true

//或是使用箭頭函式
//當程式碼只有單行時，可以省略return關鍵字和花括弧
[12, 5, 18, 11, 41].every(x => x > 10); //false
[12, 15, 18, 11, 41].every(x => x > 10); // true
```

### `find()`

會回傳**第一個**滿足測試的元素**值**，否則就回傳 `undefined`。`find()`方法不會更改原始陣列。

```javascript
const comment = comments.find((c) => c.id === 823423);
//{ text: 'Super good', id: 823423 }
```

### `findIndex()`

會回傳**第一個**滿足測試的元素**索引(index)**，否則就回傳 `-1`。`findIndex()`方法不會更改原始陣列。

```javascript
const comment = comments.findIndex((c) => c.id === 823423);
//1
```

## :beer: DEMO
