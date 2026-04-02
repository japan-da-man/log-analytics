# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

ログ分析（log-analytics）プロジェクト。

## 言語

回答・コメント・ドキュメントは原則として**日本語**で行う。

## 役割分担

- **要件定義・レビュー**: Codex（AGENTS.md・`.codex/`・`.agents/` が管轄）
- **設計〜開発**: Claude Code（このファイルが管轄）

Codex が `docs/` 配下に作成した仕様書・要件定義を正として設計・実装を行う。仕様と実装が矛盾する場合は `docs/` 配下の仕様を正とし、差分を明示する。

## 設計・実装時の方針

- 仕様が曖昧な場合は推測で実装せず、曖昧点を列挙して確認する
- 推測で補った内容は必ず「仮定」と明記する
- 破壊的変更や既存挙動を変える変更は、事前確認を行う

## Sub-Agent 構成（`.claude/agents/`）

| name | 役割 | 権限 |
|------|------|------|
| architect | 設計・境界定義・責務分割・IF/DTO/Zod/OpenAPI 整理 | read-only |
| implementer | コード実装・テスト修正・リファクタリング | read-write |
| test-designer | テストケース設計・正常系/異常系/境界値・モック戦略 | read-only |
| code-reviewer | 実装後レビュー・セキュリティ・設計逸脱検出 | read-only |
| bug-investigator | バグ調査・ログ分析・仮説列挙・再現手順特定 | read-only |
| dependency-guardian | ライブラリ妥当性検証・重複チェック・バージョン影響分析 | read-only |

## Skills（`.claude/skills/`）

| name | 呼び出し | 用途 |
|------|----------|------|
| `/design-api` | 自動 + 手動 | API 設計テンプレート（エンドポイント・スキーマ・エラー・認可・DB 影響） |
| `/implement-endpoint` | 手動のみ | 1 エンドポイント実装の定型手順（schema → handler → service → repo → route → test） |
| `/review-pr` | 自動 + 手動 | PR レビューの固定観点チェック |
| `/investigate-error` | 自動 + 手動 | エラー調査の定型手順（仮説 3 つ以上・再発防止まで） |
| `/refactor-safely` | 自動 + 手動 | リファクタリングの安全手順（public interface 維持・テスト先行） |
| `/commit-message` | 手動のみ | Conventional Commits メッセージ・PR 説明文の生成 |
| `/develop-feature` | 手動のみ | 仕様書→設計→実装→検証→自己レビュー→PR準備の一気通貫オーケストレーション |
