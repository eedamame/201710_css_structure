# 主要css設計のざっくりまとめと、私の気持ち

橘

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
レゴブロックのようにパーツを組み合わせて構築していく。

---

## 2つの基本理念

- Separate structure and skin  
- Separate container and content  

---

### Separate structure and skin
https://github.com/stubbornella/oocss/wiki#separate-structure-and-skin

基本構造とバリエーションで別クラスに切り出す。  
マルチクラスで指定することで、同じスタイルを何度も書くことを防ぐ。  
**タグレベルで書くよりも、classで指定**しておけば、新しいタグが実用的になった時にもスタイルの調整するんじゃなくて、classの付与で対応できるよね。

---

### Separate container and content
https://github.com/stubbornella/oocss/wiki#separate-container-and-content

コンテナとコンテンツを分離。場所に依存しないセレクタを書く。  
.container h2  
ではなく、 
h2.category のような感じ

上書きしたスタイルをもう一回戻すために上書きするとかしなくてすむ

---

## SMACSS
https://smacss.com/ja

---

### カテゴライズ
根底にある考え方は「カテゴライズ」
```
1. ベース
2. レイアウト
3. モジュール
4. 状態(ステート)
5. テーマ
```
レイアウトには `l-` や `layout-` 、ステートには `is-` 、テーマには `theme-` などのprefixをつける
---

### 適応性の深度
```
#sidebar div {
#sidebar div h3 {
#sidebar div ul {
```
のように、タグ構造に依存してしまってるのは変更があることを考えるとツライ。  
まず、起点となるdivからスタイルをできるように、classをつける。 `.pod`
次に、コンテンツ部分の ul は ol の場合もあるかもしれないし、単純に div かもしれない。  
なので、共通のclass `.pod-body` をつけておくとタグに依存しない指定になって柔軟性が増す。  

（ここでは全部のタグにclassをつけてclassだらけにする必要はないと言っているが、  
h3やliにもclassつけてあげたほうが詳細度あがらずにいいんじゃないかなと思う。）  

---

## BEM

http://getbem.com/
BEMはBlock, Element, Modifier の略。  
オブジェクト、コンポーネントをこの3つに分類して考える。

---

### Block
コンポーネントのベースになるクラス

### Element
Blockを構成する要素  
titleとか、btnとかかな

### Modifier
Block、Elementのバージョン違い

### MindBEMding

Block__Element--Modifier

---

## MCSS
http://operatino.github.io/MCSS/en/  
http://operatino.github.io/MCSS/ja/  
マルチレイヤーCSS  
```
- Foundation
- Base
- Project
- Cosmetic
```

---

### 読み込む順番を守る！
```
0_layer_foundation
    reset.css

1_layer_base
    base_modules.css

2_layer_project
    project_modules.css

cosmetic.css
```
先のレイヤーのスタイルから、後のレイヤーのスタイルを上書かない。  
（たとえばbaseレイヤーからprojectやcomseticレイヤーのスタイルは上書かない）  

---

## FLOCSS
https://github.com/hiloki/flocss

- Foundation
- Layout
- Object
  - Component
  - Project
  - Utility
---

## ECSS
Enduring CSS  
http://ecss.io/  

- 抽象化を避ける
- 重複を許容する

---

### property, value の繰り返しを許容する

- 冗長になってファイルサイズも増えるが、cssの多少の増減がパフォーマンスへ致命的な影響を与えるケースはまずない。
- 似たようなデザインであっても、違うUIパーツとしてスタイルを書く。
- それによって、影響範囲が特定できて、追加も安心。不要になった場合にはまるっと削除できる。

---

# 私の気持ち

難しくしたくない。  
必要なのは、

- どこに書いてあるのか、書くべきなのかがわかりやすいこと
- コードの影響範囲がわかりやすいこと （削除しやすい／追加しやすい）
- 初見でもだいたいこんな感じかなと始められること

- コードの影響範囲がわかりやすいこと
- コードの影響範囲がわかりやすいこと
