# 主要css設計のざっくりまとめと、私の気持ち

design Team 橘

---

## ざっとみてみる設計思想

- OOCSS
- SMACSS
- BEM
- FLOCSS
- ECSS

---

## OOCSS

### Object-Oriented CSS
https://github.com/stubbornella/oocss/wiki#separate-structure-and-skin

- Heavy reuse of CSS code, so less CSS code needed
- To a lesser degree, fewer repaints and layout calculations on the part of the browser.
再利用するから、コード量自体少ない。ページごとにキャッシュを使える
再描画、再計算せずに計算された数値を使いまわすことができる。

---

## OOCSS

### 2つの基本理念
#### Separate structure and skin
https://github.com/stubbornella/oocss/wiki#separate-structure-and-skin

基本構造とバリエーションで別クラスに切り出す。
マルチクラスで指定することで、同じスタイルを何度も書くことを防ぐ。
タグレベルで書くよりも、classで指定しておけば、新しいタグが実用的になった時にもスタイルの調整するんじゃなくて、classの付与で対応できるよね。

#### Separate container and content
https://github.com/stubbornella/oocss/wiki#separate-container-and-content

コンテナとコンテンツを分離。場所に依存しないセレクタを書く。
.container h2
ではなく、
h2.category のような感じ

上書きしたスタイルをもう一回戻すために上書きするとかしなくてすむ

---
