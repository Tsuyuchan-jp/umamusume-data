# umamusume-data

ウマ娘関連ツール向けの **配信用データ置き場**（生成物のみ）。

抽出スクリプトや生マスタの作業は、非公開の `umamusume-data-src` 側で行います。  
このリポジトリには、アプリが実行時に取得する JSON / 画像など **成果物だけ** を置きます。

## 方針

- リポジトリは Public（アプリ・ブラウザがログインなしで取得できるようにする）
- ソース（抽出・変換）は Private（`umamusume-data-src`）
- アプリはまず `manifest.json` を読み、差分だけ取得する想定

## レイアウト

```
manifest.json          … 目次（版・各ファイルのパスと sha256）
data/                  … JSON（キャラ・スキル・サポカなど）
assets/
  characters/          … ウマ娘アイコン
  supports/            … サポカ画像（対象分のみ）
  type-icons/          … タイプ印など共有小画像
```

## 第1波＋コース一覧（datasetVersion 0.1.4）

載せてあるもの:

- JSON: characters / skills / **supports（対象 42 件）** / events / scenarios / **courses（140 件）**
- 画像: characters 266 webp・supports 42 webp・type-icons 6 webp
- `priority-supports.json` は載せない。対象サポカ＝画像かつ events

抽出の正本は Private 工場 `umamusume-data-src`。進捗はそちら `docs/STATUS.md`。

後回し（第2波の残り）: effects（inherit・遅延読み込み）、trackData 相当、シミュ用 skill_data（工場 mdb。本家ファイルはコピーしない）

## アプリからの参照

- マニフェスト: リポジトリ直下の `manifest.json`
- 例（raw）: `https://raw.githubusercontent.com/Tsuyuchan-jp/umamusume-data/main/manifest.json`

必要に応じて GitHub Pages や jsDelivr へ切り替えても、アプリ側はマニフェスト URL だけ変えれば追従できる想定です。

## 注意

非公式のファン向けデータ配信です。ゲームの権利は権利者に帰属します。  
このリポジトリを「マスタの再配布元」として宣伝する用途は想定していません。
