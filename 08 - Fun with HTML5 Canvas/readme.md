# 08 - Fun with HTML5 Canvas

## :dart: 目標

1. 滑鼠在按著不放時可以移動滑鼠在螢幕作畫。
2. 畫筆會改變顏色與尺寸。

## :bomb: 注意

- 依視窗尺寸調整畫布大小:
  
  ```javascript
  const canvas = document.querySelector('#draw');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  ```

- 宣告一個值為布林的變數，並以監聽`mousedown`和`mouseup`和`mouseout`事件來切換布林值，來確認是否在做畫中（按著滑鼠移動）。

- 為做畫起點宣告變數，並以`offsetX/Y`屬性值賦值，可結合解構賦值使用。

- HSL色相環為360度(圓形)一循環（起終點皆為同色），因此可以隨做畫累加色相值，並在值累積為360後歸零重新計算，達到色彩循環的效果。

- 畫筆尺寸的漸大漸小可由判斷+布林切換實現。

## :old_key: 語法

### `<Canvas>`

`<Canvas>`是一個HTML元素，搭配JavaScript使用可以做出像繪圖、合成圖片、動畫等效果，本篇只會提及繪圖功能。

#### 開始

- 設定`<Canvas>`元素，並為它設定 width 與 height 屬性，如果沒有設定，預設是width=300px; height=150px;

```html
<canvas id="draw" width="800" height="800"></canvas>
```

> 為了方便於程式碼腳本找到需要的`<canvas>`，每次都設定id是一項不錯的作法。

- 建立渲染環境。最初canvas為空白，使用HTMLCanvasElement方法`getContext()`在`<canvas>`進行呼叫，並傳入參數`'2d'`，以提供繪製圖案的功能。

```javascript
const canvas = document.querySelector('#draw');
const ctx = canvas.getContext('2d');
```

#### 網格

有了畫布後，在開始作畫之前，我們得先了解畫筆是如何在畫布上定位的。畫布網格的左上角為原點(0, 0)，向右增加 x 值，向下增加 y 值。

![網格(Grid)](https://mdn.mozillademos.org/files/224/Canvas_default_grid.png)

#### 畫一條線

在現實作畫時，我們提筆、落筆、畫－－－最後收筆，在程式碼中大致上也是一致的。

- `beginPath()`：提筆，產生一個新路徑。
- `moveTo(mX, mY)`：落筆，指定路徑起點。
- `lineTo(lX, lY)`：從指定點(mX, mY)畫一條直線路徑到這點(lX,lY)。
- `stroke()`：將肉眼看不見的路徑填充成可見的線。

```javascript
let lastX = 0;
let lastY = 0;
function draw(e) {
  ctx.beginPath();
  ctx.moveTo(lastX, lastY);
  ctx.lineTo(e.offsetX, e.offsetY);
  ctx.stroke();
}
canvas.addEventListener('mousemove', draw);
```

#### 顏色與樣式

- `strokeStyle = color`：設定勾勒路徑的顏色。color以字串表示，寫法可以是：
  - 關鍵字，例如：`blue`。
  - RGB立體座標，例如：`#808000`、`rgb(128,128,0)` 和 `rgba(128,128,0,0.5)`。
  - HSL圓柱座標，例如：`hsl(186, 100%, 50%)` 和 `hsla(120,100%,75%,0.3)`。
- `lineWidth = value`：設定線條寬度，必須為正數，預設值為1.0單位。
- `lineCap = type`：設定線條結尾的樣式。
  - `butt`：線條端點樣式為方形。
  - `round`：線條端點樣式為圓形。
  - `square`：增加寬度與線條寬度相同、高度為寬度一半的的方塊於線條端點。

    ![端點樣式](https://mdn.mozillademos.org/files/236/Canvas_linecap.png)
- `lineJoin = type`：設定線條和線條間接合處的樣式。
  - `round`：代表圓弧型連接樣式，接合處為圓角。
  - `bevel`：代表斜面型連接樣式，接合處為平角。
  - `miter`：預設值，代表斜交型連接樣式，接合處為尖角。

    ![接合處樣式](https://mdn.mozillademos.org/files/237/Canvas_linejoin.png)

```javascript
ctx.strokeStyle = 'red';
ctx.lineJoin = 'bevel';
ctx.lineCap = 'square';
ctx.lineWidth = 100;
```

### mouse事件

事件 | 描述
--- | ---
mousedown | 使用者在元素上按下滑鼠按鍵時發生
mousemove | 使用者在元素上移動滑鼠時發生
mouseup | 使用者在元素上放開滑鼠按鍵時發生
mouseout | 使用者將滑鼠移出元素時發生

各種 (x, y) 座標比較
屬性 | 時點| 回傳什麼 | 備註
--- | --- | --- | ---
clientX/Y | mouse事件 | 滑鼠游標位於目前視窗的水平/垂直座標 | client:瀏覽器窗口的大小
movementX/Y | `mousemove`事件 | 滑鼠游標位於最後一個發生`mousemove`事件位置的水平/垂直座標 |
offsetX/Y | mouse事件 | 滑鼠游標位於目標元素邊緣位置的水平/垂直座標 |
pageX/Y | mouse事件 | 滑鼠游標位於document的水平/垂直座標 | page:整個網頁內容
screenX/Y | mouse事件/window | 滑鼠游標位於螢幕的水平/垂直座標 | screen:螢幕解析度的大小

## :beer: DEMO
