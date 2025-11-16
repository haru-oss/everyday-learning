# everyday-learning
日々の学びを更新していく


🧪 前提（あなたの現在の初期値）


```tsx
const [products, setProducts] = useState([
  { productName: "", quantity: 1 }
]);
```

UI 上は最初から 1 個だけフォームが見えている。

⸻

🔥 ケース：setProducts([…products, 新しいやつ]) を使っていると壊れる流れ

⸻

① 初期状態（画面を開いた直後）

```tsx
products = [
  { productName: "", quantity: 1 }
]

```

② ユーザーが 1 個目を入力

入力:
```tsx
productName = "YQM"
quantity = 2

```
↓
React 内部の state は次の通り：



```tsx
products = [
  { productName: "YQM", quantity: 2 }
]

```
③ ユーザーが “+ 商品追加” を押す（1回目）

あなたがもし以下を使っていた場合：

```tsx
setProducts([...products, { productName:"", quantity:1 }]);
```

ここで 問題が起こる可能性がある。

理由：React の state 更新がまだ反映されていない可能性がある

つまり内部的にはこうなる可能性がある：

🧨 React がまだ更新前の products を使ってしまうケース

React の内部はまだ古い状態の可能性あり：

```tsx
products = [
  { productName: "", quantity: 1 }   ← 古い！！！ (YQM に更新される前の状態)
]
```

そして新規追加するので：

```tsx
products = [
  { productName: "", quantity: 1 },
  { productName: "", quantity: 1 }
]
```

④ ユーザーが2個目の商品を入力（DWSSH 3）
```tsx
products = [
  { productName: "", quantity: 1 },  ← 本当は YQM 2 のはず
  { productName: "DWSSH", quantity: 3 }
]
```

★ 1個目が YQM ではなく、空欄に戻ってしまってる！

ユーザーから見える UI 上は変更が反映されているように見えるが、
React の state 中身は ズレたまま。

⸻

⑤ ユーザーが “+ 商品追加” を押す（2回目）

まだ state がズレており、最新ではない可能性あり：

React 内部の products（ズレた状態）が使われる：
```tsx
[
  { "", 1 },
  { "DWSSH", 3 }
]
```
追加されると：

```tsx
[
  { "", 1 },
  { "DWSSH", 3 },
  { "", 1 } ← 3個目
]

```

🧟‍♂️ 結果として起きる悲劇

あなたが書いた通り：

✔ UI 上は全部問題ない

（ユーザーはちゃんと入力している）

✔ しかし内部 state には「反映されていない古いデータ」が混じる

（YQMが消える、順番が狂う、空欄が復活する）

✔ 最終的に送信されるデータは壊れている

⸻

🙅‍♂️ 結論：連打しなければ OK…ではない

❌「連打しなければ大丈夫」は残念ながら不正解

理由：
	•	React はユーザー操作とは関係なく
「バッチ処理（まとめて更新）」を勝手に行う
	•	連打しなくても、
『非同期である限りズレる可能性は常にある』

だから React 公式が
「配列やオブジェクトの state 更新では callback form を使え」
と強く推してる。



```tsx

```









