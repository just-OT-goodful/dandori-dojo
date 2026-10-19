# だんどり道場 — 開発者向けメモ (CLAUDE.md)

60〜70歳の脳卒中患者向けの遂行機能トレーニングアプリ。作業療法士がつき添い、主に**iPad**で使う。
`index.html` 1枚で完結（ビルドなし）。GitHub Pages：https://just-ot-goodful.github.io/dandori-dojo/

## 画面づくりの方針
- **ゲーム画面はスクロールせず1画面に収める**（お手本や地図を見ながら操作するため）。
- 大人向けのトーン・デザインを保つ。

## 古いSafari（古いiPad）対応の決まりごと（2026年10月19日に実機相当の古いWebKitで総点検）

古いiPad（iOS 12止まりの iPad Air 1・mini 2/3、iOS 13〜15.3 の iPad）で、
「枠からはみ出す」「スクロールしないと選択肢が出ない」「迷路がなぞれない」が起きていた。原因と決まり：

1. **`env()` や `var()` を含む宣言は、古いブラウザでも「有効」と判定されて前の代替指定を上書きし、
   計算できないと「指定なし」になる。** 例：`height:calc(100dvh - 16px - env(safe-area-inset-top))`
   は iOS 11.2〜15.3 で高さが消え、ゲーム画面が最小460pxに縮んでいた（全ゲームの崩れの主因）。
   → `dvh`・`clamp()` などの新しい書き方を `var()`/`env()` と同じ宣言に混ぜない。
   → ゲーム画面の高さは JS（`setH`）が `window.innerHeight` から px で直接入れている。
   → `vh` は古いiOSではツールバーを除いた高さにならないため、JS が入れる `--vh`（実寸の1%）を使う。
     `clamp()` と `var()` を組み合わせるときは `max(a,min(b,c))` の形にする（iOS 11.3以降で動く）。
2. **上下を固定した枠（`.area>*` は position:absolute）の中の grid は、古いWebKitが行の高さを中身から決めてしまう。**
   → `autoFit()` が `.area` の子に高さを px で入れている。新しいゲームでもそのまま効く。
3. **grid の中の position:absolute の要素（○×表示など）が、古いWebKitでは場所を取って中身がずれる。**
   → 中央寄せは grid の place-items ではなく flex（align-items/justify-content）で。
4. **インラインSVGは width だけでなく height も指定する**（height:auto を古いWebKitが誤計算する）。
5. **刺激の大きさは画面の高さではなく枠の実寸に合わせる**（`fitStage()`、迷路は `fitMaze()`）。
   ResizeObserver が無い iOS 13.0以前のため、`watchFit()` は表示直後・文字の読み込み後にも合わせ直す。
6. **指の操作は touch イベントで受ける**（`onDown/onMove/onUp`）。iPadOS 13〜14 は PointerEvent はあるが、
   SVGの上では `touch-action:none` が効かず、なぞる途中で pointercancel になり迷路のポインターが動かなかった。
   マウス・トラックパッドだけ pointer で受ける（Apple Pencil も touch 側。二重反応を防ぐ）。
7. **レベルが上がって中身が増えても1画面に収める**：`autoFit()` が収まらないときに段階的に詰め表示にする
   （おかいもの：棚を詰める→2列→合計・レジを棚の下へ、お出かけ計画・よていづくり：`tight`→`tight2`）。
   新しく項目が増える画面を作ったら `FITS` に登録する。
8. **絵文字はiOSのバージョンで表示できるものが違う。** iOS 12までの端末では Emoji 12 以降が□になる
   （例：🧅🧄🦥）。Emoji 11（🥬など）は iOS 12.1以降。新しい絵文字を使うときは確認すること。
9. flexの `gap` は iOS 14.5未満で効かないため、`no-flexgap` のときは補助処理が margin で代わりを作る。

## 確認方法（実機が無いので）

おぼえてタッチのリポジトリ（just-OT-goodful/oboete-touch）の `tools/oldwebkit/` に、
**本物の古いWebKit**（Ubuntu 18.04 の WebKitGTK 2.20 ≒ iOS 11.3〜12、20.04 の 2.28 ≒ iOS 13〜14）で
画面を撮影・測定する道具がある。`setup.sh` で環境を作り、`sweep.sh` で全12ゲーム×レベル1〜5を
iPadのSafari表示サイズ（横1024×661・縦768×917、ツールバーとタブ表示時）で、はみ出し・スクロール・
枠内スクロール・JSエラーを自動判定する。

```bash
sudo tools/oldwebkit/setup.sh                       # 初回のみ（root・Ubuntuのパッケージサーバー接続が必要）
cp index.html /opt/wk/work/dd/index.html
LV=1,2,3,4,5 /opt/wk/work/tools/sweep.sh dd /work/dd/index.html   # 結果の "NG" 行が問題箇所
```
それでも本物のiPadではないので、最後は実機での確認をユーザーに依頼すること。
