# 03 - CSS Variables

## :dart: 目標

1. 調整畫面中的滑桿/顏色時，畫面同步產生對應效果

## :bomb: 注意

- 如何取得`<input>`值？
  - 監聽`change`事件，當使用者更改`<input>`或`<select>`或`<textarea>`元素時呼叫函式
    > `change` 與 `input`類似，但是`change`是發生在元素失去焦點時內容產生了變化； `input`是在內容產生變化後立即發生
  - 監聽`mousemove`事件，當滑鼠在元素上移動時呼叫函式。
    > 例外元素：`<base>`、`<bdo>`、`<br>`、`<head>`、`<html>`、`<iframe>`、`<meta>`、`<param>`、`<script>`、`<style>` 和 `<title>`
  - 監聽`input`事件，當使用者更改`<input>`或`<textarea>`元素時呼叫函式

- 如何取得對應CSS屬性名稱與數值、單位？
  - 名稱：`name` attribute -> `this.name`
  - 數值：`value` attribute -> `this.value`
  - 單位：`data-sizing` attribute -> `this.dataset.sizing || ''`
    > `dataset`屬性提供一組字符串Map。可以設定多組data-*屬性，並在`element.dataset.屬姓名`來進行讀取(無法寫入) [參考](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)

- 如何改變CSS屬性值？
  - `document.documentElement.style.setProperty(propertyname, value, priority)`
    > 在這個練習中是改變CSS的全域變數
    >
    > 延伸思考：宣告CSS變數的位置，會決定此變數的可見性。因此通常CSS變數會宣告在`:root{}`偽類選擇器中或是`html{}`選擇器中。我們使變數存在於父元素中，並讓子元素可以看見使用。
    >
    >但是反過來說，當我們把變數宣告在子元素中，並在父元素使用此變數時，這種情況父元素是**無法**獲得存在於子元素的變數值的！ [參考](https://blog.logrocket.com/css-variables-scoping/)

## :old_key: 語法

### CSS 變數

宣告方式：以兩個減號(--)加上屬性名，屬性值可以是任何CSS的有效值。通常最佳的宣告位置是在`:root`下，也可依需求調整。

```css
:root {
    --main-bg-color:blue;
}
```

使用方式：以`var()`包覆變數。

```css
div{
    background-color: var(--main-bg-color);
}
```

### CSS 濾鏡

```css
img{
    filter: blur(10px);
}
```

- none; **無效果**
- blur(px); **模糊**
- brightness(%); **亮度**
- contrast(%); **對比度**
- drop-shadow(h-shadow v-shadow blur spread color); **陰影效果**
- grayscale(%); **灰階**
- hue-rotate(deg); **色相旋轉**
- invert(%); **負片**
- opacity(%); **透明度**
- saturate(%); **飽和度**
- sepia(%); **懷舊**

## :beer: DEMO
