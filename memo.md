## AHC027 memo

- 掃除をして平均汚れを最小化する問題
- 出来るだけ最短で全てのマスを通る経路を求める ⇒ 同じ手数の経路で以下に下げられるか みたいな感じで 2 つの問題に分ける？

- 基本寄り道はデバフ ⇒ 最小の寄り道の経路にすべし
- ハミルトングラフであればハミルトン路を採用すべき
- そうでないなら最小の寄り道でハミルトン路に近い未知を採用すべき

- ↑ むずすぎでは？ハミルトンの存在確認でも NP 困難、しかもある可能性超低い
- 盤面を直方体に区切る ⇒ 少なくとも縦横どちらかが偶数長だと元に戻ってこれることを利用？
- 直方体選別は左上になり得るもの全探索 (左上が壁 or 既に訪れ) ⇒ 最大長方形を抜く

- 貪欲は完成 ⇒ 途中で汚れが多いところだけを重点的に自分で操作したところスコアが凄く上がった

### 貪欲続き

1. まず全てのマスをさらう
2. 期待値が良いマスがある (汚れ/マンハッタン距離 が (全ますの汚れ増加分 \* 距離) より大きい場合、そのマスに行くことを繰り返す)
3. 95000 ターン or TLE になりそうになるまで続けて最後ゴールに戻る

- 1 をしてる最中にも期待値が良いものがあったら行くようにしたら結構行けそう？
- というか『一度も訪れていない』ということも評価値に入れて、一番評価が高いマスに逐次行くのが良さそう？
- それでも最後に訪れてないマスは愚直に行くとか？
- 幅 1 のビームサーチっぽい感じ？(ちょっと調べてみるべし)

- 幅 1 Chokudai サーチはやってみる価値あり。
- 各マスの評価値候補

1. ( (汚れ) + (未訪問 ? 10000 : 0) ) / (マンハッタン距離)

- 上下左右どちらかに行くかは、上下左右のマスの評価値の平均が一番良い方向に行く
- 全部訪問したら打ち切って start に戻り、best_state の更新処理
- 問題は 汚れを保持するとメモリで死ぬ点。未訪問は bit で持てばメモリは大丈夫そう。
  ⇒ まずは評価値 ( 未訪問 ? (汚れ)\*turn : 0 ) / (マンハッタンとしてみる)
- 深さ 10000, 幅 1 の chokudai search で愚直判定 O(N^2) でやってみる
