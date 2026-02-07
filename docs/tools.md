# プロジェクト固有ツール設定

> このファイルはインフラ情報を管理します

## 🔗 開発環境URL

| 環境 | URL | メモ |
|------|-----|------|
| Google Apps Script | Google Apps Scriptエディタ | スプレッドシートから開く |
| Web App（認証用） | デプロイ後に生成 | 認証機能用エンドポイント |

## Git管理
- リポジトリ: https://github.com/{user}/gas-task-manager
- ブランチ戦略: main（本番）、develop（開発）

## Google Apps Script
- プロジェクト: gas-task-manager
- デプロイ: Web App
- 認証: メールアドレスベース

## MCP Servers
- **filesystem**: プロジェクトファイルへのアクセス

### Claude Code 設定
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/kitamuranaohiro/Private/P dev/gas　販売作成"]
    }
  }
}
```

## CLIツール
- `gh`: GitHub CLI（✅ インストール済み）
- `clasp`: Google Apps Script CLI（⚠️ 手動インストール必要）

### claspのインストール
```bash
sudo npm install -g @google/clasp
clasp login
```

## セットアップ完了日
2026-02-07
