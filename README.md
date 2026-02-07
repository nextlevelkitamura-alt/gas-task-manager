# GAS Task Manager（統合配布版）

多くの人がGoogleスプレッドシートで本格的なタスク管理を実現できる、認証機能付き販売用GASアプリ

## 特徴

### 🔐 認証システム
- メールアドレスベースの認証
- 7日間キャッシュによるパフォーマンス向上
- Web Appによる配布・認証管理

### 📋 タスク管理
- プロジェクト別のシート自動作成
- タスクID自動生成
- 完了タスクの自動アーカイブ
- チェックボックスによる簡単操作（スマホ対応）

### 📅 カレンダー連携
- Googleカレンダーとの双方向同期
- 未設定タスクの自動登録
- カレンダー変更の自動反映

### 🛠️ 保守機能
- 設定診断（checkConfiguration）
- レイアウト修復（repairCheckboxes）
- 自動トリガー設定

## セットアップ

### 1. clasp のインストール
```bash
sudo npm install -g @google/clasp
clasp login
```

### 2. GitHubリポジトリのクローン
```bash
git clone https://github.com/{user}/gas-task-manager.git
cd gas-task-manager
```

### 3. GASプロジェクトの作成
```bash
clasp create --type sheets --title "GAS Task Manager"
clasp push
```

### 4. スプレッドシートの設定
1. 生成されたスプレッドシートを開く
2. メニューから「📅 タスク連携」>「設定: 自動化トリガーをセット」を実行
3. 認証リストシートにメールアドレスを追加

## 開発

詳細は `docs/ROADMAP.md` を参照してください。

## ライセンス

販売用プロダクト - All Rights Reserved
