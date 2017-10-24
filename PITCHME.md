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

再利用するから、コード量自体少ない。ページごとにキャッシュを使える。  
再描画、再計算せずに計算された数値を使いまわすことができる。  
レゴブロックのようにパーツを組み合わせて構築していく。

+++

### 基本構造とバリエーションで別クラスに切り出す。  
マルチクラスで指定することで、同じスタイルを何度も書くことを防ぐ。  
**タグレベルで書くよりも、classで指定**しておけば、新しいタグが実用的になった時にもスタイルの調整するんじゃなくて、classの付与で対応できるよね。

+++

### コンテナとコンテンツを分離
場所に依存しないセレクタを書く。  
`.container h2`  
ではなく、 
`h2.category`

---

## SMACSS
https://smacss.com/ja

+++

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
+++

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

+++

### Block
コンポーネントのベースになるクラス

### Element
Blockを構成する要素  
titleとか、btnとかかな

### Modifier
Block、Elementのバージョン違い

+++

### MindBEMding

Block__Element--Modifier

---

## MCSS
http://operatino.github.io/MCSS/en/  
http://operatino.github.io/MCSS/ja/  

マルチレイヤーCSS  

- Foundation
- Base
- Project
- Cosmetic

+++

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

+++

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

+++

- OOCSS のように、タグではなくclassベースで、構造に依存しないようにスタイルの指定をしていき、|
- SMACSS のように、カテゴライズをして、適切なカテゴリに記述することで管理しやすくして、|
- BEM のように、class名のルール付をしていくことで、自然とコンポーネント指向なclass名の設計をして、|
- MCSS のように、読み込み順の制御でレイヤー間の影響範囲をわかりやすくし、|
- FLOCSS のように、細分化したルールでかっちりやればうまくいくのではと思いつつ難しいので諦めて、|
- ECSS のように、可能な限り、影響範囲を閉じ込めたカスケーディング／汎用パーツ化しないようにすることで、|

+++

今のところうまくいっているように思っています。

---

## ファイル構成

- scssフォルダ
  - styles.scss : エントリポイント。@importのみする。
  - _setting.scss : 変数、mixinなど。mixinが増える場合は `abstract.scss` を作る時も。
  - _layout.scss : その名の通り、大枠のレイアウトに関するcss。
  - _module.scss : ECSS的に〜とはいいつつ、ボタン／タイトルなどのそれだけで成り立つミニマムなものはモジュール化したい
  - _page.scss : 個別ページ用の指定。ここから@importする。この下は、極力ECSS的なアプローチをとる。
    - _top.scss : トップページ用
    - _about.scss : 会社概要用〜〜などなど

---

## その他気をつけていること

個人の好みです。が、参考まで。

- よっぽどわかりやすいものでなければmixinにしない
  - clearfixとか、mediaqueryとかはOK。
- extendは使わない。
  - 書くコード量は減るが、読む時に追いかけるのが極端に面倒になる。

---

ありがとうございました。
