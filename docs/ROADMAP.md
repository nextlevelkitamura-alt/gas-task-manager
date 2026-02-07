# GAS Task Manager（統合配布版）
> 多くの人がGoogleスプレッドシートで本格的なタスク管理を実現できる、認証機能付き販売用GASアプリ

## 技術スタック
- **プラットフォーム**: Google Apps Script (GAS)
- **データ**: Google Sheets
- **連携**: Google Calendar
- **デプロイ**: Web App（認証機能付き）

## インフラ
- **Git**: https://github.com/nextlevelkitamura-alt/gas-task-manager
- **配布方法**: スプレッドシートのコピー配布
- **認証**: メールアドレスベースの認証システム

## 実装方針
- ロジック: Claude Code（/tdd, /d）
- UI: Google Sheets（既存テンプレート）
- 配布: Web App + スプレッドシートコピー

## 現在のフェーズ: Phase 1 - 統合作業

## 機能一覧

### 必須（統合する機能）

#### 🔧 認証gasとプライベートgasの統合 → [計画書](plans/features/gas-integration.md)
以下の機能を含む統合版GASを作成：
- メールアドレス認証システム（doGet、checkUserPermission）
- 7日間キャッシュによる認証
- 設定診断機能（checkConfiguration）
- レイアウト修復機能（repairCheckboxes）
- プロジェクト管理（プロジェクト一覧からシート自動作成）
- タスク管理・ID自動生成
- カレンダー同期（双方向）
- チェックボックス機能（スマホ操作対応）
- 完了処理・アーカイブ
- ダッシュボード機能（タスク一覧）
- 未設定タスクのカレンダー登録
- カレンダー取込・振り分け機能

#### 配布用の準備
- ○ 最終確認・動作テスト（統合後）

### 削除する機能
- ✗ タスク集計・分析機能（aggregateAndVisualize、ANALYSIS_SHEET_NAME）

### 拡張（将来的な機能）
- ○ タスクのタグ機能
- ○ モバイル最適化
- ○ 通知機能の強化
- ○ チーム機能（複数ユーザー）

## 完了履歴
（まだありません）
