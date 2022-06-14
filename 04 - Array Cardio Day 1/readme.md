# 04 - Array Cardio Day 1

## :dart: 目標

熟悉幾種JS陣列方法：`filter()`、`map()`、`sort()`、`reduce()`

## :bomb: 注意

- 使用`console.table()`，以表格狀態呈現數據，數據必須為陣列或物件。

- 使用三元運算子使條件句更簡潔。`condition ? exprIfTrue : exprIfFalse`

- 使用`Array.from()`或`[...arraylike]`(展開運算符)，將一個類陣列（array-like）或是可迭代（iterable）物件建立一個新的陣列。
  > 展開運算符(Spread Operator)與其餘運算符(Rest Operator)都是`...`
  >
  > 在ES9後`...`也可作用在物件上(rest/Spread properties)

- 使用解構賦值(destructuring)

  ```javascript
  const [x, y, z] = [1, 2, 3]
  console.log(x) //1
  ```

  > 解構賦值與其餘運算符很常搭配使用

  ```javascript
  const [x, ...y] = [1, 2, 3]
  console.log(x) //1
  console.log(y) //[2,3]
  ```

## :old_key: 語法

### filter()

會得到一個符合條件(callback function 的回傳值為true)後的新陣列。

`let newArray = arr.filter(callback(element[, index[, array]])[, thisArg])`

```javascript
//一般函式版本
const fifteen = inventors.filter(function(inventor){
  if(inventor.year >= 1500 && inventor.year < 1600){
    return true;
  }else{
    return false;
  }
});

//箭頭函式版本
//當箭頭函式裡的內容只有'return'的時候，我們可以拿掉return和外面的大括號
const fifteen = inventors.filter(
  (inventor) => inventor.year >= 1500 && inventor.year < 1600
);
```

### map()

建立一個新的陣列，內容為原陣列的元素經由回呼函式運算後，所回傳的結果。如果是不需要回傳新陣列的情況，則要使用`forEach()`。

```javascript
const fullNames = inventors.map(
  (inventor) => `${inventor.first} ${inventor.last}`
);
```

### sort()

會將陣列做排序。預設的排序順序是根據字串的 Unicode 編碼位置（code points）而定。

```javascript
const arr = [1, 30, 4, 21, 100000];
arr.sort();
console.log(arr);
// [1, 100000, 21, 30, 4]
```

`sort()`可以接受一個compareFunction，作為排序的標準。如果compareFunction的回傳值是 `-1` ，陣列將以升冪排序；若回傳值是`1`，則陣列將以降冪排序。除此之外，當比較值為數字時，也可將compareFunction的回傳值寫作`a - b`，陣列將以升冪排序。

```javascript
const ordered = inventors.sort(function (a, b) {
  if (a.year > b.year) {
     return 1;
  } else {
    return -1;
  }
});

//或

const ordered = inventors.sort((a, b) => (a.year > b.year ? 1 : -1));
```

如果想減少排序的運算，可以搭配`map()`使用。[參考](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#example:_sorting_maps)

### reduce()

將陣列的所有元素傳入回呼函式，回呼函式處理每一個元素後並累加，最後得到一項累加值。`reduce()`的第二個參數則為累加器的初始值。

回呼函數的第一個參數為累加器，第二個元素為目前元素。

`arr.reduce(callback[accumulator, currentValue, currentIndex, array], initialValue)`

當回呼函式第一次呼叫時，如果有提供`initialValue`值，`accumulator`將等於`initialValue`，且 `currentValue` 會等於陣列中的第一個元素值；若沒有提供 `initialValue`，則 `accumulator` 會等於陣列的第一個元素值(array[0])，且 `currentValue` 將會等於陣列的第二個元素值(array[1])。

**故提供`initialValue`是較為安全的作法**

```javascript
const totalYears = inventors.reduce((total, inventor) => {
  return total + (inventor.passed - inventor.year);
}, 0);
```

`reduce()`可以統計陣列中值的重複次數，並將結果存入一個物件。

```javascript
const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck', 'pogostick'];

const sum = data.reduce(function(obj, item) {
  if (!obj[item]) {
    obj[item] = 0;
  }
  obj[item]++;
  return obj;
}, {});
```

## :beer: DEMO
