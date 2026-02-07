---
feature: 認証gasとプライベートgasの統合
type: integration
method: manual
created: 2026-02-07
status: planning
---

# 設計プラン: 認証gasとプライベートgasの統合

## ROADMAP 上の位置づけ

Phase 1 - 統合作業の中核タスク。
認証gasとプライベートgasの2つの既存ファイル（合計約4,000行）を1つの統合版GASに統合する。

## 目的

販売可能な「GAS Task Manager（統合配布版）」を作成する。
- 認証機能付きタスク管理システム
- Googleスプレッドシートで動作
- Googleカレンダーとの双方向同期
- メールアドレスベースの認証・配布

## コード分析結果

### ファイル構成
- **認証gas**: 1,996行、26関数
- **プライベートgas**: 1,959行、23関数
- **合計**: 約4,000行

### 統合方針
**ベース**: 認証gasを採用（認証機能を含むため）

### 機能の取捨選択

#### 認証gasから採用する独自機能
1. ✅ **doGet()** - Web App認証エンドポイント
2. ✅ **checkUserPermission()** - 認証チェック（7日間キャッシュ付き）
3. ✅ **checkConfiguration()** - 設定診断ツール
4. ✅ **repairCheckboxes()** - レイアウト修復機能
5. ✅ **VERSION定数** - バージョン管理
6. ✅ **AUTH_URL定数** - 認証用WebアプリURL

#### プライベートgasから削除する機能
- ❌ **aggregateAndVisualize()** - タスク集計・分析（不要）
- ❌ **ANALYSIS_SHEET_NAME定数** - タスク集計シート（不要）

#### 共通機能（22関数）
両方のファイルに存在する機能は、**認証gasのバージョンを採用**（より新しいと想定）。

### 差分の詳細

#### 認証gas独自の実装
```javascript
// 1. Web App認証
function doGet(e) { ... }

// 2. 認証チェック（マスターSSとキャッシュ対応）
function checkUserPermission(silent = false) { ... }

// 3. 設定診断
function checkConfiguration() { ... }

// 4. レイアウト修復
function repairCheckboxes() { ... }

// 5. バージョン管理
const VERSION = '2.0.0';
const AUTH_URL = "https://script.google.com/.../exec";
```

#### プライベートgas独自の実装（削除対象）
```javascript
// タスク集計・分析（削除）
function aggregateAndVisualize() { ... }
const ANALYSIS_SHEET_NAME = 'タスク集計';
```

#### setupTriggers() の違い
**認証gas**: レイアウト修復トリガーを追加
```javascript
// 5. レイアウト修復: 毎日4時
ScriptApp.newTrigger('repairCheckboxes')
  .timeBased()
  .atHour(4)
  .everyDays(1)
  .create();
```

**プライベートgas**: レイアウト修復トリガーなし

→ **認証gasのバージョンを採用**

#### onEditTrigger() の違い
**認証gas**:
- `checkUserPermission(true)` で権限チェック実施
- タスク集計シートの処理なし

**プライベートgas**:
- 権限チェックなし
- タスク集計シートの処理あり（削除対象）

→ **認証gasのバージョンを採用**

## リスク評価

### HIGH リスク
- **コードの巨大さ**: 約4,000行の統合は手動作業でミスのリスク
- **テスト環境**: 実際のスプレッドシートでの動作確認が必須
- **認証URL**: 既存のAUTH_URLをそのまま使うか、新規デプロイするか要確認

### MEDIUM リスク
- **トリガーの重複**: setupTriggers() を実行時にトリガーが正しく設定されるか
- **シート構造**: 認証リストシートの存在が前提

### LOW リスク
- **定数の統一**: ほぼ同一のため問題なし
- **関数名の衝突**: 共通関数は同名のため問題なし

## 依存関係

### 必須
- Google Apps Script環境
- Google Sheets
- Google Calendar API
- SpreadsheetApp、CalendarApp、PropertiesService、Session等のGAS API

### 外部サービス
- 認証用Web App（AUTH_URL）- デプロイ済みと想定

### スプレッドシート構成（必須シート）
1. プロジェクト一覧
2. プロジェクトテンプレート
3. タスク入力
4. 設定
5. 完了タスク
6. タスク一覧
7. カレンダー取込
8. gas管理
9. **認証リスト**（認証gasで必須）

## 実装フェーズ

### Phase 1: ファイル準備と差分確認 ✅
- [x] 認証gasの関数リスト抽出
- [x] プライベートgasの関数リスト抽出
- [x] 定数の差分確認
- [x] 統合方針の決定

### Phase 2: 統合版ファイルの作成
- [ ] 認証gasをベースファイルとしてコピー
- [ ] プライベートgasから削除対象を確認
  - aggregateAndVisualize()関数
  - ANALYSIS_SHEET_NAME定数
  - onEditTriggerのタスク集計処理
- [ ] EXCLUDED_SHEETSから'タスク集計'を除外
- [ ] VERSION を 3.0.0 に更新（統合版として）

### Phase 3: コメント・ドキュメントの整理
- [ ] ファイルヘッダーコメントを更新
  - 「統合版」であることを明記
  - 削除した機能を記録
- [ ] 機能一覧コメントを更新
- [ ] 認証機能の説明を追加

### Phase 4: 統合版の保存
- [ ] 新ファイル `統合版.gas` を作成
- [ ] Gitコミット

### Phase 5: 手動テスト準備（実装後）
- [ ] Googleスプレッドシート作成
- [ ] 必須シート9枚の作成
- [ ] 認証リストにテストユーザー追加
- [ ] GASエディタに統合版をコピー
- [ ] setupTriggers() 実行
- [ ] 各機能の動作確認

### Phase 6: 配布用準備（最終）
- [ ] Web Appとしてデプロイ
- [ ] AUTH_URLを新しいURLに更新（必要に応じて）
- [ ] README作成
- [ ] 配布用テンプレートシート作成

## 実装対象ファイル

### 作成
- `統合版.gas` - 認証gasとプライベートgasの統合版（約2,000行）

### 変更
- `認証gas` - 参照元（変更なし）
- `プライベートgas` - 参照元（変更なし）

### 削除
- なし（既存ファイルは保持）

## 統合後の構成

```
統合版.gas (約2,000行)
├── 定数定義（認証gasベース）
│   ├── VERSION = '3.0.0'
│   ├── AUTH_URL
│   └── その他定数
├── 認証システム（4関数）
│   ├── doGet()
│   ├── checkUserPermission()
│   ├── checkConfiguration()
│   └── repairCheckboxes()
└── 基本機能（22関数）
    ├── onOpen()
    ├── setupTriggers()
    ├── onEditTrigger()
    ├── タスク管理系
    ├── カレンダー同期系
    ├── ダッシュボード系
    └── その他
```

## 推奨実装方式

→ **手動統合（manual）**

理由:
- GASは単一ファイルで約2,000行程度が適切
- テストはGoogle Apps Scriptエディタで直接実行
- /tdd や /impl は適用困難（GAS環境の制約）

## 次のステップ

1. **統合版ファイルの作成** - Phase 2を実行
2. **手動テスト** - Googleスプレッドシートで動作確認
3. **配布準備** - テンプレート作成とドキュメント整備

## 注意事項

### AUTH_URLについて
現在のAUTH_URL:
```
https://script.google.com/macros/s/AKfycbymabLBIHrbYwxlICC2IYxTStaK7XJ6RFUB4AP1fHqRj83d7gchtQlMfkBVOtSAFX2dOw/exec
```

このURLは認証gasのデプロイ先と思われる。統合版でも同じURLを使うか、新規デプロイするか要確認。

### 配布方法
1. スプレッドシートのテンプレートを作成
2. ユーザーが「コピーを作成」で複製
3. 認証リストに自分のメールアドレスを追加
4. setupTriggers() を実行して利用開始

### 販売時の考慮事項
- 認証リストの初期設定方法を明記
- セットアップガイドの作成
- Web Appデプロイ手順の文書化
