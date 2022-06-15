# 05 - Flex Panel Gallery

## :dart: 目標

當滑鼠點到圖片上時，圖片會張開，上下文字會出現。

## :bomb: 注意

- `>` 直屬選擇器(child combinator)，只匹配`selector1`直接後代的`selector2`。 ~~我都記要兒不要孫~~

  ```css
  selector1 > selector2 { style properties }
  ```

- 文字位移效果：`transform: translateY();`

- 為了呈現圖片先張開，文字再出現的效果，有兩個重點：
  - 分別監聽`click`和`transitionend`事件
  - `transitionend`事件中回呼的函式內需判斷`flex`屬性出現後，才加上文字出現的CSS。

## :old_key: 語法

### Flexbox

Flexbox是一種盒模型，他能使元素自動適應畫面大小。

#### 建立一個flexbox

再父元素上加入`display: flex;` (或是`display:inline-flex`)將元素變成彈性容器，子元素在預設的情況下，會依照自身大小從左自右緊貼排列。之後，可以依需求調整方向的橫直，如何分布，各自佔據畫面比例等等…

#### 排列方向

在flexbox的世界，我們不會稱水平（inline）或垂直（block），而是主軸（main axis）與切軸（cross axis）。透過改變`flex-direction`的值，我們可以改變主軸與切軸的方向
有四種排列子元素的方式：

- row：預設值，（主軸）從左到右，（切軸）再從上到下排列。
- row-reverse：與 row 相反。
- column：（主軸）從上到下，（切軸）再由左到右。
- column-reverse：與 column 相反。

#### 對齊

分為主軸(justify-content)和切軸(align-*)排列，於父元素定義來決定子元素如何對齊，下方**簡略**介紹常用幾種。

- 主軸

```css
justify-content: center;     /* 置中排列 */
justify-content: start;      /* 從頭部開始排列 */
justify-content: end;        /* 從尾部開始排列 */
justify-content: space-between;  /* 均匀排列每個元素，頭尾貼合邊緣 */
justify-content: space-around;  /* 均匀排列每個元素，頭尾間距是元素間距的一半 */
justify-content: space-evenly;  /* 均匀排列每個元素，間距平均分配 */
```

- 切軸

切軸有區分`align-items`與`align-self`，後者是定義元素自身（設定在子元素上）的排列，可以覆蓋前者。

```css
/* align-items */
align-items: center;     /* 置中排列 */
align-items: start;      /* 從頭部開始排列 */
align-items: end;        /* 從尾部開始排列 */

/* align-self */
align-self: center;     /* 置中排列 */
align-self: start;      /* 從頭部開始排列 */
align-self: end;        /* 從尾部開始排列 */
```

#### 順序

可以在子元素中設定`order: X`，來改變子元素排列的順序，預設值是0。

#### 比例

`flex`屬性決定了子元素佔據了父元素多少比例，他可以再細分成**3**種屬性，依照順序分別是：`flex-grow`，`flex-shrink`，`flex-basis`，三者值皆為數字或auto。

- `flex-grow`：決定了所佔容器中的，**剩餘空間**的相對比例。預設值為 0，不會進行彈性變化，不可為負值。設為 1 （或 > 1）則會進行彈性變化，子元素們會填滿整個畫面，不留下剩餘空間。

- `flex-shrink`：只有在子元素尺寸和大於父元素尺寸時生效，會依數值決定子元素收縮比例，不可為負數。預設值為 0

- `flex-basis`：子元素的基本大小，決定了内容盒（content-box）的尺寸，優先度比`width`還要高。下方說明數值的意義：
  - `auto`：呈現自身的原尺寸。
  - `0`：預設值，無效自身本身尺寸，直接採用 `flex-grow` 屬性。
  - `X px`：決定自身尺寸。

#### 換行

當使用flexbox時，子元素以單行的方式彈性撐滿父元素，但若希望子元素可以換行，則可設定：

- nowrap：預設值，單行。
- wrap：多行。
- wrap-reverse：多行，但內容元素反轉。

## :beer: DEMO
