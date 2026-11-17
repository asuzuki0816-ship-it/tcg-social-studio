# tcg-social-studio

トレカ（トレーディングカード）を海外に販売する店のための、毎日の SNS 投稿づくりを支援する AI エージェント用スキルです。
商品の調査から、投稿の制作（リール／カルーセル／画像）、英語本文と日本語訳の作成、検品、Google スプレッドシートの運用表への記入、そして Instagram / Facebook への投稿までを、決まった流れで進めます。

> **In short (English):** An AI-agent skill that runs a daily social-media workflow for Japanese trading-card shops selling overseas — product research, post production (reels / carousels / images), bilingual captions (English + Japanese), quality checks, tracking in a Google Sheet, and posting to Instagram & Facebook. Only posts a human has approved in the sheet get published.

## 主な機能

- **毎日の3案制作**：商品を調査し、リール・カルーセル・画像の3形式で投稿案を完成させる
- **バイリンガル本文**：英語の投稿本文と、確認用の日本語訳を必ずセットで作る
- **運用表で管理**：Google スプレッドシートの「運用表」に、完成物・本文・選んだ理由・数値の記録までを一元管理
- **人がチェックした案だけ投稿**：表の「投稿OK」にチェックが付いた行だけを、決まった時刻に Instagram / Facebook へ投稿する（チェックの無い行は投稿しない）
- **ストーリーズ同時投稿**：フィード投稿と同時に、専用サイズ（1080×1920）のストーリーズ版も出す
- **固定ページ・固定ハッシュタグ**：毎回の投稿の最後に付ける案内ページやハッシュタグを設定で統一できる
- **検品の仕組み**：数字の裏付け、価格の出典記録、文字の読みやすさなどをチェックリストで確認してから表に書く

## 必要なもの

- スキル（SKILL.md）を読んで実行できる AI エージェント環境
- Node.js 22 以上、Python 3（`requirements.txt` / `requirements-analysis.txt` のパッケージ）
- Google アカウント（スプレッドシートとドライブを運用表・共有フォルダに使う）
- 投稿先の Instagram アカウント / Facebook ページ
- ブラウザ操作の環境（投稿や調査に使う。初回に Chrome を自動で用意する仕組み付き）

## セットアップ

詳しい手順は [START_HERE.md](START_HERE.md) にあります。流れだけ書くと：

1. このフォルダを、使っている AI エージェントのスキル置き場に置く
2. `client.example.json` を `client.json` にコピーし、店名・アカウント名・保存先など自分の情報を入れる
   （スプレッドシートと共有フォルダは、アドレスを空にしておくと初回に AI が自動で作ります）
3. 依存パッケージを入れる
   ```
   python -m pip install -r requirements.txt
   python -m pip install -r requirements-analysis.txt
   npm install
   ```
4. `node scripts/check_env.mjs` と `node scripts/check_client.mjs` で準備を確認する

`client.json` は個人設定ファイルなので、このリポジトリには含めていません（`.gitignore` でも除外しています）。

## 使い方の流れ

1. **制作**：毎日決まった時刻（または「今日の投稿を作って」の指示）で、調査→制作→検品→運用表への記入までを自動で進める
2. **チェック**：運用表を見て、投稿したい案の「投稿OK」にチェックを入れる（ここだけは人が行う）
3. **投稿**：決まった時刻に表を確認し、チェックの付いた行だけを1回に1本ずつ投稿し、結果を表に書き戻す

## 注意事項（運用ルールの例）

このスキルは「作るのは自動、出す判断は人」という分担を前提にしています。実際の運用で決めているルールの例：

- **投稿OK のチェックは人だけが付ける。** AI はチェックを付けないし、チェックの無い行を投稿しない
- **価格の出典は必ず記録する。** 相場（市場価格）を投稿に出すときは、出典・価格の種類・確認日を運用表に残し、画面にも短い出典注記を1か所だけ入れる（「その数字の根拠は？」に答えるため）
- **公式価格・定価（希望小売価格）は投稿に出さない。** 出してよい価格は相場だけ、という決め方もできる（店の方針に合わせて `client.json` の `do_not` に書く）
- **日本語訳は確認用。** 投稿本文（英語）の内容確認のために作るもので、投稿そのものには混ぜない
- 完成物のリンクは、表の中で実際にクリックして開ける形で書く

## フォルダ構成

| 場所 | 内容 |
|---|---|
| `SKILL.md` | スキル本体（毎日の進め方） |
| `START_HERE.md` | 初回の準備手順 |
| `client.example.json` | 設定ファイルの見本（架空の値） |
| `references/` | 調査・制作・文章・検品・表・投稿などの進め方 |
| `scripts/` | 環境チェック、プレビュー作成、動画仕上げなどの道具 |
| `templates/` | スライドとリールのひな形 |
| `fonts/` / `se-pack/` | 同梱フォントと効果音（ライセンスは `fonts/FONT_LICENSES.md`） |
| `sheets-template/` | 運用表を自作するときの CSV ひな形 |
| `CHANGES.md` | 変更履歴 |

## ライセンス・利用について

利用は自由です。ただし、実際の店舗運営・投稿に使う場合は自己責任でお願いします（アカウントの安全や投稿内容の最終確認は、利用する方ご自身で）。フォント・効果音など同梱素材の権利は、それぞれのライセンスに従ってください。

---

Originally developed for a Japanese TCG shop's overseas sales operation, and shared so other shops and creators can adapt it to their own workflow.
