# roy-log (ロイログ)

XAU/USD のトレード配信(ロイさんのシグナル)を貼り付けて記録し、価格チャートと照合するための個人用ツール。単一ファイルの静的サイト。

## 構成

- `index.html` — アプリ本体(HTML/CSS/JSを1ファイルに全部書いている。ビルド不要)
- `icon.svg` / `icon-180.png` — PWA用アイコン
- GitHub Pagesが`main`ブランチの`index.html`をそのまま配信 → https://hoshi2.github.io/roy-log/

## 主な機能

- 「貼る」タブ: 配信テキストをパースしてロング/ショートのゾーン・条件ライン・付近価格・MA条件・利確ターゲットを自動抽出(`parseRoy()`)、LINEのトーク履歴(.txt)からの一括インポートにも対応(`parseLineExport()`)
- 「タイムライン」タブ: 保存した配信を日付ごとに一覧表示、TwelveData APIで実際の値動き(必要ならMAクロスも実計算)と照合してステータス判定。冒頭に成績サマリー(今月/全期間/方向別の発動率・TP到達率)を表示。過去日の判定結果は`royStatusFinal`に確定保存して再取得しない
- 「チャート」タブ: lightweight-charts でローソク足を表示し、ゾーン/TPをオーバーレイ
- 「設定」タブ: TwelveData APIキー、Firebaseクラウド保存(コレクション`roylog`)、JSONバックアップ

## データ保存

- ブラウザの`localStorage`が正(オフラインでも動く)
- Firebase接続時は`roylog`コレクションの、ユーザーが決めた「同期コード」をドキュメントIDにしてJSON丸ごと保存し、端末間で同期。習慣アプリ(p26)・借金管理アプリ(STELLA FINANCE)と同じ方式(それぞれ`p26`/`stella`コレクションを使う横並びの実装)

## 開発メモ

- 外部依存はCDN読み込みのみ(lightweight-charts, TwelveData REST API, Firebase compat SDK)。npm/ビルドは無し
- 変更は`index.html`を直接編集するだけでOK
