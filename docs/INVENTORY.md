# 既存アプリのデータ／画像 棚卸し

作成日: 2026-08-22（skill-emulator 追記同日）  
目的: `umamusume-data`（Public 配信用）に何を載せるか判断するための一覧。  
対象: `C:\Users\PC1\Projects` 配下 ＋ `Documents\google antigravity\umamusume-skill-emulator`。  
※ このファイルのコピーを `umamusume-data/docs/INVENTORY.md` にも置く（ワークスペースから読めるようにする）。

---

## 1. 結論（先に読む）

### ハブ（`umamusume-data`）に載せる価値が高いもの

| 種別 | 中身の目安 | 現在の正本 | 利用者 |
|------|------------|------------|--------|
| キャラ JSON | 約 262 件 | **sp-calc** `data/characters.json` | sp-calc, inherit-skill-list, title-call-quiz（派生） |
| スキル JSON | 約 2127 件 | **sp-calc** `data/skills.json` | sp-calc, inherit-skill-list |
| サポカ JSON | 約 547 件 | **sp-calc** `data/supports.json` | sp-calc, inherit-skill-list |
| イベント JSON 一式 | 優先サポカイベント等 | **sp-calc** `data/events*.json` | sp-calc, inherit-skill-list |
| 優先サポカ一覧 | 40 件 | **sp-calc**（events から生成） | sp-calc, inherit-skill-list |
| シナリオ（トレセン軒） | 小 | **sp-calc** / inherit に同内容 | sp-calc, inherit-skill-list |
| キャラアイコン | 262 webp / 約 2.9 MB | **sp-calc** `assets/characters/`（inherit・title-call がコピー） | 上記＋title-call-quiz |
| 優先サポカ画像 | 40 webp / 約 0.9 MB | **sp-calc** `assets/supports/` | sp-calc, inherit-skill-list |
| タイプ印 | 6 webp | **sp-calc** `assets/type-icons/` | sp-calc（主） |

**正本は事実上 `umamusume-sp-calc`。** inherit / title-call はここからコピーしている。

### ハブ候補だが「別枠・後回し」でよいもの

| 種別 | 場所 | 理由 |
|------|------|------|
| コース一覧 | inherit `data/courses.json`（U-tools） | マスタ系と更新源が違う |
| コース別有効スキル | inherit `data/effects/**`（約 6.8 MB） | 大きく、更新手順も別（U-tools） |
| コース抽出 JSON | `uma-course-extract/output/` | mdb 由来だが用途が狭い |
| **umasim フルスキル効果** | skill-emulator `skill_data.txt`（~1.6 MB・2066件） | sp-calc の skills と別系統。AGPL（umasim）注意 |
| skill-emulator コース | `trackData.ts`（137 コース） | inherit courses と突合余地あり・ソース別 |

### ハブに載せなくてよいもの

| アプリ | 理由 |
|--------|------|
| rental-factor-fill | マスタなし。貼り付けテキスト＋ウマ娘DB DOM のみ |
| receipt-capture | マスタなし。トレイアイコン＋実行時キャプチャのみ |
| nige-kensai-report | 静的調査 HTML のみ |
| renso | 知識ベース。ゲームデータなし |
| oshi-keiba | 現状シードのみ（将来スキル接続時に再検討） |
| sp-calc の `assets/ui/`・各アプリ favicon | アプリ固有 UI |
| title-call の `assets/voices/` | ボイス。データハブとは別管理が自然 |
| `_tmp-support-card-sp` | sp-calc 前身のレガシー。新規ハブの正本にしない |
| skill-emulator の WASM / OpenCV / golden PNG / master.mdb | アプリ密結合・再配布不適 |

**重要:** skill-emulator は「JSON が重い・重要」だが、**sp-calc 系カード／イベント正本とは別物**。第1波ハブの供給元にはならない。

---

## 2. アプリ別サマリ

| アプリ | マスタ JSON | カード画像 | 更新の手間 | ハブ優先度 |
|--------|-------------|------------|------------|------------|
| **umamusume-sp-calc** | ◎ 正本 | ◎ 正本 | 高（mdb + U-tools + 画像） | **最高（供給元）** |
| **umamusume-inherit-skill-list** | ◎ sp-calc 流用 + U-tools effects | ◎ sp-calc 流用 | 高（マスタ追従＋ effects） | **高（最初の移行先候補）** |
| **umamusume-skill-emulator** | ◎ umasim スキル効果（別系統） | カード画像なし | 高（umasim + mdb rarity） | **中（第2波・ライセンス注意）** |
| **uma-title-call-quiz** | roster（派生） | キャラのみコピー | 中 | **中（アイコン共有）** |
| **uma-course-extract** | コース専用 | なし | 中 | 低〜中（別パッケージ可） |
| **umamusume-rental-factor-fill** | なし | なし | — | なし |
| **umamusume-receipt-capture** | なし | アプリアイコンのみ | — | なし |
| **oshi-keiba** | 将来の可能性 | — | — | 将来 |
| **_tmp-support-card-sp** | 埋め込み・古い | あり（旧） | — | アーカイブ |
| **nige-kensai / renso** | なし | なし | — | なし |

---

## 3. 詳細：umamusume-sp-calc（正本）

パス: `C:\Users\PC1\Projects\umamusume-sp-calc`

### ランタイムで読むデータ

| パス | 件数の目安 | 内容 |
|------|------------|------|
| `data/skills.json` | 2127 | スキル |
| `data/supports.json` | 547 | サポカ全件 |
| `data/characters.json` | 262 | 育成カード・覚醒スキル |
| `data/events.json` | イベント 111 / 優先 40 | 優先サポカイベント |
| `data/scenarios/toresenken.json` | 小 | トレセン軒 |

### パイプライン用（アプリ非読込〜生成物）

`meta.json`, `priority-supports.json`, `events.extracted.json`, `events.preserve.json`, `events.default-overrides.json`, `events.id-aliases.json`, 各種 report など。

### 画像

| パス | 枚数 | サイズ目安 |
|------|------|------------|
| `assets/characters/*.webp` | 262 | ~2.9 MB |
| `assets/supports/*.webp` | 40 | ~0.9 MB |
| `assets/type-icons/*.webp` | 6 | 極小 |
| `assets/ui/uma-world.png` | 1 | UI 固有 → ハブ外で可 |

### 読み方

- ビルドバンドルなし。`fetch("../data/...")` と相対パス画像。
- 更新は手元スクリプト → **リポジトリにコミットして Pages 配信**（今の更新忘れの主因）。

### 主な更新コマンド（概念）

- `npm run extract` … master.mdb → skills / supports / characters
- `extract:events` / `apply:events` … U-tools + mdb → events
- `assets:extract` / `assets:import` … ゲーム dat → webp

### 外部ソース

- ゲーム `master.mdb` / meta / dat（例: DMM インストール配下）
- U-tools（サポカイベント）
- 一部 GameWith・umasim 参考

---

## 4. 詳細：umamusume-inherit-skill-list

パス: `C:\Users\PC1\Projects\umamusume-inherit-skill-list`

### sp-calc と同系統（コピー／流用）

- `data/skills.json`, `supports.json`, `characters.json`, `events.json`, `priority-supports.json`
- `data/scenarios/toresenken.json`
- `assets/characters/`（262）, `assets/supports/`（40）

→ **ハブ化すれば二重メンテをやめられる本命。**

### このアプリ独自（更新源が U-tools）

| パス | サイズ目安 | 内容 |
|------|------------|------|
| `data/courses.json` | ~28 KB / 140 コース | コース一覧 |
| `data/effects/available.json` | 小 | 効果 JSON があるコース |
| `data/effects/{courseId}/{style}.json` | 合計 ~6.8 MB | 脚質別の有効白スキル等 |

スクリプト: `extract:courses` / `extract:effects`（このリポ内）。  
マスタを mdb から作り直すスクリプトは **このリポには無い**（sp-calc 依存）。

---

## 5. 詳細：umamusume-skill-emulator（重要・別系統）

パス: `C:\Users\PC1\Documents\google antigravity\umamusume-skill-emulator`  
（`Projects` 配下ではないため、初回棚卸しから漏れていた）

本番はほぼ `desktop-app/`（Next.js + Tauri）。レース分析・OCR・キャプチャ結合。

### 「JSON が重い」の正体

素の `.json` は少ない。本体は次の二重管理。

| 資産 | パス | サイズ／件数 | 用途 |
|------|------|--------------|------|
| **フルスキル効果** | `desktop-app/public/skill_data.txt`（中身は JSON 配列） | **~1.6 MB / 2066 件** | レース sim（条件・効果・持続） |
| 簡略スキル | `desktop-app/app/lib/umasimSkillData.ts` | ~183 KB / 2066 件 | OCR・パレット（name/id/rarity） |
| レアリティ色 | `app/lib/skillRarityData.ts` | ~1857 件 | UI 色（master.mdb 由来） |
| コース | `app/lib/trackData.ts` | 競馬場 17 / コース 137 | レース条件 UI |
| プリセット | `public/race_presets.json` | 3 件 | UI |

### ランタイムの取り方（スキルフルデータ）

1. GitHub raw（`mee1080/umasim` の `skill_data.txt`）を fetch  
2. 失敗時は AppData キャッシュ  
3. それでもダメなら同梱 `/skill_data.txt`  

→ **すでに「起動時取得」に近い**。sp-calc 系の「リポにコミットして Pages」問題とは別パターン。

### カード画像・sp-calc 系マスタ

- **キャラ／サポカ webp・events・supports JSON は無し**
- sp-calc の `skills.json`（~2127・baseSp 等）と umasim の 2066 件は **スキーマも用途も別**
- 重なるのは「スキル」「コース」という名前レベルのみ

### アプリ固有の重いもの（ハブ非推奨）

| 資産 | 目安 | 理由 |
|------|------|------|
| `public/umaemu/*.wasm` 等 | ~0.7 MB + js | AGPL・バージョン密結合 |
| `public/opencv.js` | **~10.7 MB** | 画像結合専用 |
| `auto-capture-design-pack/golden/` | PNG ~27 MB | テスト／設計用 |
| ルート `master.mdb` | ~42 MB | 抽出入力。再配布不適（gitignore 記載ありつつ追跡残の可能性） |

### 更新手段

- `npm run extract-umasim-skills` … umasim → `skill_data.txt` + `umasimSkillData.ts`
- `npm run extract-skill-info` … `master.mdb` → rarity TS
- コース／プリセットは手動寄り
- 外部正本: **mee1080/umasim**（AGPL）＋ ゲーム mdb（色のみ）

### ハブへの意味

- **第1波（sp-calc カード一式）には混ぜない**
- 載せるなら第2波以降の **`umasim` スキル効果** として別キー（ライセンス明記必須）
- 今すぐの一元化メリットは、sp-calc↔inherit の画像／マスタほど大きくない（すでに GitHub fetch があるため）

---

## 6. 詳細：その他

### uma-title-call-quiz

- `data/roster.json`（charaId / name / iconCardId）
- `assets/characters/{iconCardId}.webp` … **sp-calc からコピー**
- `assets/voices/*.ogg` … アプリ固有（ハブ必須ではない）

### umamusume-rental-factor-fill

- ゲームデータなし。samples は貼り付け例のみ。
- inherit の出力をウマ娘DBに流す連携のみ。

### umamusume-receipt-capture

- `assets/icon.ico` 等のみ。マスタ・カード画像なし。

### uma-course-extract

- `output/courses/*.json` 等。コースイベント系。アイコンなし。
- ハブに載せるなら `data/courses/` の別セクション向き。

### _tmp-support-card-sp

- HTML 埋め込みマスタ＋旧画像。sp-calc へ移行済み前提のレガシー。

### oshi-keiba / nige-kensai / renso

- 現状、共有マスタ配信の対象外（oshi は将来の可能性のみ）。

---

## 7. データの流れ（現状）

```text
master.mdb / ゲーム dat
        │
        ▼
umamusume-sp-calc     ← カード／イベント正本（JSON + 画像）
        │ 手コピー
        ├──────────────► umamusume-inherit-skill-list
        │                      │
        │                      ├ U-tools → courses / effects（独自）
        │                      └ コピー結果 → rental-factor-fill（テキスト連携）
        └──────────────► uma-title-call-quiz（キャラ画像・roster）

mee1080/umasim (skill_data.txt + WASM)
        │ fetch / 同梱
        ▼
umamusume-skill-emulator  ← レース sim 用スキル効果（別系統）
        │
        └ master.mdb → rarity 色のみ
```

目指す形（第1波）:

```text
master.mdb / dat / U-tools
        │
        ▼
umamusume-data-src（Private・工場）
        │ 生成して push
        ▼
umamusume-data（Public・棚）
        │ fetch
        ├─ sp-calc
        ├─ inherit-skill-list
        └─ title-call-quiz（画像など）

（第2波・任意）umasim スキル効果 → skill-emulator も同じ棚を見る
```

---

## 8. ハブ初版に入れる推奨セット

骨格は既に `umamusume-data` にある。中身を載せるときの第1波:

1. `data/characters.json`
2. `data/skills.json`
3. `data/supports.json`
4. `data/events.json`（および運用上必要な付随 JSON は要検討）
5. `data/priority-supports.json`
6. `data/scenarios/toresenken.json`
7. `assets/characters/*.webp`
8. `assets/supports/*.webp`
9. `assets/type-icons/*.webp`
10. `manifest.json` を実ファイルに合わせて更新

**第2波（別キーでよい）:**

- `data/courses.json` / `data/effects/**`（inherit・U-tools）
- umasim 系 `skill_data`（skill-emulator・**AGPL 明記**）
- skill-emulator `trackData` 相当のコース（突合できれば courses に統合検討）

**ハブに入れない:**

- UI 背景・favicon・voices・receipt アイコン
- skill-emulator の WASM / OpenCV / golden / master.mdb
- `.cache`・抽出レポート・gitignore 済み生キャッシュ

---

## 9. 重複の実態（メンテコストの正体）

同じ内容が複数リポに居るもの:

| 資産 | sp-calc | inherit | title-call | skill-emulator |
|------|---------|---------|------------|----------------|
| skills / supports / characters（mdb 系） | ◎ | ◎ コピー | roster のみ | — |
| events / priority-supports | ◎ | ◎ コピー | — | — |
| characters webp | ◎ | ◎ コピー | ◎ コピー | — |
| supports webp | ◎ | ◎ コピー | — | — |
| courses / effects（U-tools） | — | ◎ 独自 | — | — |
| umasim skill_data（効果付き） | — | — | — | ◎（＋ GitHub fetch） |

ゲーム更新のたびに **sp-calc を更新 → inherit / title-call へコピー忘れ** が起きやすい構造。  
skill-emulator のスキル更新は **umasim 追従**が主で、上記コピー問題とは別軸。

---

## 10. 次にやるとよい作業順

1. この棚卸しを前提に、`manifest.json` の `files` / `assets` を第1波に合わせて確定する  
2. sp-calc の該当ファイルを `umamusume-data` に載せる（初回はコピーで可）  
3. 接続実験は **inherit-skill-list か sp-calc の一方だけ**  
4. 抽出スクリプトを `umamusume-data-src` へ移す／ラッパを置く  
5. courses/effects・umasim skill_data は需要とライセンスを見て第2波  

---

## 11. 調査対象パス一覧

| パス | 役割 |
|------|------|
| `...\Projects\umamusume-sp-calc` | カード／イベント正本 |
| `...\Projects\umamusume-inherit-skill-list` | 流用＋ U-tools effects |
| `...\Documents\google antigravity\umamusume-skill-emulator` | umasim スキル効果・レース sim |
| `...\Projects\uma-title-call-quiz` | キャラ画像流用 |
| `...\Projects\umamusume-rental-factor-fill` | データなし |
| `...\Projects\umamusume-receipt-capture` | データなし |
| `...\Projects\uma-course-extract` | コース抽出 |
| `...\Projects\oshi-keiba` | 将来候補 |
| `...\Projects\_tmp-support-card-sp` | レガシー |
| `...\Projects\umamusume-nige-kensai-report` | 対象外 |
| `...\Projects\renso` | 対象外 |
| `...\Projects\umamusume-data` | Public 棚（骨格済） |
| `...\Projects\umamusume-data-src` | 工場・本ドキュメントの編集元 |
