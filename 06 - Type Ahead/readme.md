# 06 - Type Ahead

## :dart: 目標

在輸入關鍵字後，下方會出現符合關鍵字的：

1. 地點(city, state)，並且符合關鍵字的部分需要height ligth。
2. 人口(population)，並以逗號分隔。

## :bomb: 注意

- 在`fetch()`取得資料後，想存入變數時，會因變數不同的宣告方式而有不同的存入方法：
  - 用`let`宣告時，直接賦值。

    ```javascript
    let cities = [];
    fetch(endpoint).then((res) =>
      res.json().then((data) => cities = data
    );
    ```

  - 用`const`宣告時，需使用`push()`方法後，並使用`...`運算子將陣列展開。(不能重複指定值的緣故)

    ```javascript
    const cities = [];
    fetch(endpoint).then((res) =>
      res.json().then((data) => cities.push(...data))
    );
    ```

- 在輸入框監聽兩種事件`change`和`keyup`

  > `keyup` 與`keydowm` 的差異在於：
  > `keyup`是在放開鍵盤的剎那觸發，故同一鍵只會觸發一次。
  > `keydowm`是在按下鍵盤的剎那觸發，故同一鍵按著不放時，會連續多次觸發。

- 在使用`map()`處理想要渲染到畫面的資料後、使用`innerHTML`前，記得需`join()`資料，將資料格式從陣列轉成字串。

  ```javascript
  function displayMatches() {
    const matchArray = findMatches(this.value, cities);
    const html = matchArray
      .map((place) => {
        const regex = new RegExp(this.value, 'gi');
        const cityName = place.city.replace(
          regex,
          `<span class="hl">${this.value}</span>`
        );
        const stateName = place.state.replace(
          regex,
          `<span class="hl">${this.value}</span>`
        );
        return `
        <li>
          <span class="name">${cityName}, ${stateName}</span>
          <span class="population">${numberWithCommas(
            place.population
          )}</span>
        </li>
      `;
      })
      .join('');
    suggestions.innerHTML = html;
  }
  ```

- 額外思考：嘗試對數據進行排序和搞懂地理位置（geolocation）是如何工作的。

## :old_key: 語法

### Fetch

`fetch()`回傳的 promise 不會 reject HTTP 的 error status，就算是 HTTP 404 或 500 也一樣。相反地，它會正常地 resolve，並把 ok status 設為 false。會讓它發生 reject 的只有網路錯誤或其他會中斷 request 的情況(像是遇到CORS或server設定錯誤)。

#### 發送請求

流程大致上為：
fetch發送請求 -> then收到響應後 -> 須將回傳內容的格式解析成JSON -> then將取得的資料存入變數/加工/呈現等等...

```javascript
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```

#### 上傳資料

需使用`fetch()`的第二個參數，可以選擇使用的參數包含：

- method: 請求使用的方法，如GET、POST。
- headers: 請求的頭。
- body: 請求的主體。注意 GET 或 HEAD 方法的請求不能包含主體信息。
- mode: 請求的模式，如cors、no-cors或者same-origin。
- credentials: 請求的cookies。
- cache: 請求的快取模式。
- redirect: 請求的重定向模式。
- referrer: 默認是client。
- referrerPolicy: HTTP 頭部引用指定了的值。
- integrity：請求的子資源完整性值。

```javascript
var url = 'https://example.com/profile';
var data = {username: 'example'};

fetch(url, {
  method: 'POST', // or 'PUT'
  body: JSON.stringify(data), // data can be `string` or {object}!
  headers: new Headers({
    'Content-Type': 'application/json'
  })
}).then(res => res.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

### 正規表達式

這邊只有找一些比較基礎的(?)知識，更加深入的技巧就…之後再說了:upside_down_face:

#### 建立正規表達式

在Javascript中，有兩種建立正規表達式的方式：

- 正規表達式實質/字面值(literal)：通常用在已知內容時，於載入階段編譯。

  ```javascript
  const pattern = /test/;
  ```

- RegExp 物件實例/建構器：通常用在需要動態生成內容時，於執行階段編譯。

  ```javascript
  const pattern = new RegExp('test');
  ```

#### 旗幟(Flag)

旗幟用在正規表達式實質的後方`/test/ig`或RegExp建構器的第二引數`new RegExp('test, 'ig')`，每個旗幟都有不同的意義。

- `i`：讓正規表達式不區分大小寫。
- `g`：匹配所有符合樣式的地方。
- `m`：允許對多行文字做匹配。
- `y`：進行黏性匹配。
- `u`：對Unicode數值碼做跳脫。

#### 詞彙(term)和運算子(operator)

##### 完全符合

任何字元，只要不是特殊字元和運算子，出現在正規表達式中，字元們就也得同樣地出現在字串裡，才算是符合樣式。

##### 匹配字元集合

當我們想符合的字元不是單一字元，而是在字元集合中，有任一符合的字元時，我們可以使用集合運算子`[]`。

舉例來說，當正規表達式為`[abc]`時，只要字串裡有a或b或c時都算通過。同時`[abc]`也可以寫成`[a-c]`表示a至c都任意字元。而`[^abc]`則可以表示abc**以外**的任何字元。

##### 跳脫

並不是所有的字元都只代表了他們自己，有些字元還代表了其他含意。而要使用這個"其他含意"時，就要來用反斜線`\`跳脫。例如：

- `\d`：匹配任何數字，等同於`[0-9]`。
- `\w`：匹配任何拉丁文數字元和底線符號，等同於`[A-Za-z0-9-]`。
- `\W`：(大寫W)匹配`\w`(小寫w)不匹配的所有字元。
- `\s`：匹配空白字元。
- `\S`：(大寫S)匹配所有非空白字元。

而`\`用在特殊符號時，則可以使特殊字元不使用他的特殊功能，而是代表字元本身。（例如：`\[`，則可代表"["本身）

##### 頭尾

當我們希望字串的開頭或結尾，恰好就是我們期望的字元時，可以用`^`、`$`。

- `^`：代表開頭，例如：`/^test/`，只會符合以test開頭的字串（像是'testsomethimg'）。
- `$`：代表結尾，例如：`/test$/`，只會符合以test結尾的字串（像是'somethimgtest'）。

當同時使用`^`和`$`時，字串頭尾就必須完全一致了。

##### 重複

當我們希望字元重複出現(固定次數或是n次以上)，我們可以用下列特殊字元：

- `?`：加在字元後方，代表字元可以出現**0次**或**1次**。例如：`/a?bc/`就符合'abc'或'bc'。
- `+`：加在字元後方，代表字元可以出現**1次**或**1次以上**。例如：`/a+bc/`就符合'abc'或'aaabc'。
- `*`：加在字元後方，代表字元可以出現**任意次**。例如：`/a*bc/`就符合'abc'或'bc'或'aaaabc'。
- `{}`：加在字元後方，代表字元重複出現**`{`n`}`次**。例如：`/a{3}bc/`就符合'aaabc'。
  - `{}`可以配合`,`使用表示重複次數的範圍。例如：`/a{1,3}bc/`a可以重複出現1~3次。
  - 若是像`{n,}`省略逗號後的數字，則會變成重複n次或n次以上。

在預設情況下，這些重複運算子是貪婪的(greedy)，它會獲取匹配的最大值。而在重複運算子加上`?`會使運算子變成不貪婪模式(nongreedy)，以獲取可匹配項目，例如：

'aaa'在`/a+/`的情況下，他會匹配'aaa'所有的a字元；而'aaa'在`/a+?/`的情況下，他會匹配'a'，只需要一個a字元。

##### 分組

在上述的運算子裡，一個運算符只能為一個字元服務，如果我們讓運算符可以為一組詞彙來服務時，我們可以用`()`來包裹詞彙使用，例如：`(ab)+`可以匹配'ababab'這樣的字串。

##### 多重選項

可以用管道字元`|`來表示多重選項(或)，例如：`a|b`可以符合'a'或是'b'字串。

#### 字串方法結合正規表達式使用

可以在多種字串方法中使用正規表達式，以達到像是尋找特定字元並取代的效果。對本次練習所使用方法`match()`，`replace()`做簡單說明。

##### `match()`

- 使用旗幟`g`：回傳一個陣列，其中包含所有匹配的字串；如果沒有匹配則回傳`null`。
- 不使用旗幟`g`：和方法`exec()`相同，回傳一個捕獲群組。

```javascript
function findMatches(wordToMatch, cities) {
  return cities.filter((item) => {
    const regex = new RegExp(wordToMatch, 'gi');
    return item.city.match(regex) || item.state.match(regex);
  });
}
```

##### `replace()`

回傳一個替換過值的字符串。`str.replace(regexp|substr, newSubStr|function)`，第一個參數表示那些內容要換，第二個參數表示要換成什麼內容。

```javascript
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}
//\B 只匹配非單詞邊界
//(?=<<pattern>>) 正向預看(positive lookahead)：只有在pattern 符合接著而來的東西時，才會匹配。pattern只用來預先查看(look ahead)，不會產生匹配。
//(?!<<pattern>>) 負向預看(negative lookahead)：只有在pattern 不符合接著而來的東西時，才會匹配。pattern只用來預先查看(look ahead)，不會產生匹配。
```

#### 參考

- 忍者：JavaScript開發技巧探秘
- 簡明完整的JS精要指南

練習正規表達式的好去處：

- [regular expressions 101](https://regex101.com/)
- [RegExr](https://regexr.com/)

## :beer: DEMO
