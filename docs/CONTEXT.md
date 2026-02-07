# プロジェクト全体像

> このファイルは /next で自動更新されます

## 目的
多くの人がGoogleスプレッドシートで本格的なタスク管理を実現できる、認証機能付き販売用GASアプリ

## 技術スタック
- Google Apps Script (GAS)
- Google Sheets
- Google Calendar
- Web App デプロイ

## インフラ
- Git: https://github.com/nextlevelkitamura-alt/gas-task-manager
- 配布: スプレッドシートコピー + Web App

## 現在の状況
Phase 1 - 統合作業（設計完了、実装開始可能）

進捗: 0/2 (0%)
- 🔧 実装中: 認証gasとプライベートgasの統合
- ○ 未着手: 配布用の準備

## 実装中
**認証gasとプライベートgasの統合**
- 計画書: docs/plans/features/gas-integration.md
- 方式: 手動統合
- 規模: 約4,000行 → 約2,000行に統合

## ファイル構成
- `認証gas`: 1,996行（認証機能を含む完全版）
- `プライベートgas`: 1,959行（基本機能全て）
- `配布用 gas`: 最終確認用

## 次のアクション
→ Phase 2: 統合版ファイルの作成を開始
→ 認証gasをベースに、不要な機能を削除して統合版を作成

最終更新: 2026-02-07 22:30
