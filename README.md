# だんどり道場

脳卒中後の方（60〜70歳代を想定）向けの、遂行機能トレーニングアプリです。
ブラウザだけで動く1ファイルのWebアプリで、iPadでの使用を想定しています。

## 特徴

- **Goal Management Training（GMT）の流れ**：どの課題も「とまる → 目標 → 計画 → 実行 → 確認」の順に進みます
- **メタ認知の練習**：始める前に「どのくらいできそうか」を予想し、終わったあとに結果と比べてふりかえります
- **12種類の課題**（4つの力 × 3種類）

| 鍛える力 | 課題 |
|---|---|
| 計画 | ボールの塔（ロンドン塔課題）／てじゅんならべ／めいろ |
| 段取り | お出かけ計画（Multiple Errands Test）／よていづくり／おかいもの |
| 切り替え | こうごタッチ（Trail Making Test）／いろ？かたち？／かずの切りかえ |
| がまん | ゴー・ストップ（Go/No-Go）／いろよみ（ストループ）／はんたいタッチ |

- **れんぞく稽古**：5・7・10分のあいだ、12種類の課題を次々にクリアしていくモード
- 5段階のレベルが結果に合わせて自動で上下します
- 効果音・読み上げ・BGM（任意）つき
- 複数の利用者を切り替えて、レベルと記録を別々に保存できます

## 使い方

`index.html` をブラウザで開くだけで動きます。GitHub Pagesを有効にすると、URLからiPadで開けます。

## 注意

- 記録は、使っている端末のブラウザの中（localStorage）だけに保存されます。
- このアプリは検査（評価）の代わりにはならず、医療機器ではありません。療法士が隣について使うことを想定しています。

## 主な文献

- Levine B, et al. J Int Neuropsychol Soc. 2000（GMT）
- Stamenova V, Levine B. Neuropsychol Rehabil. 2019（GMTのメタ分析）
- Cicerone KD, et al. Arch Phys Med Rehabil. 2019（認知リハビリテーションの系統的レビュー）
- Miyake A, et al. Cogn Psychol. 2000（遂行機能の統一性と多様性）
- Shallice T. Philos Trans R Soc Lond B. 1982（ロンドン塔課題）
- Shallice T, Burgess PW. Brain. 1991（Multiple Errands Test）
