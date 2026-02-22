[English](README.md) | [繁體中文](README.zh.md)

# changelog-gen

將 git commit 歷史轉換為精緻的、面向客戶的版本說明，分類變更並過濾內部雜訊。

## 概述

將原始 git commits 轉換為專業的版本說明，聚焦於面向使用者的變更：
- 解析指定範圍的 commits
- 按類型分類（功能、修復、改善、重大變更）
- 過濾內部雜訊（CI、測試、重構）
- 將技術訊息改寫為客戶友善的語言

## 工作流程

### 1. 確定範圍
指定 commit 範圍：
- 「自上次發布以來」
- 「自 v2.4.0 以來」
- 「最近 2 週」
- 「v1.0 到 v2.0 之間」

### 2. 分類
根據 conventional commit 前綴映射 commits：
- `feat:` / `feature:` → 新功能
- `fix:` / `bugfix:` → 修復
- `improve:` / `enhance:` / `perf:` → 改善
- `BREAKING CHANGE` / `!:` → 重大變更
- `security:` → 安全性

### 3. 過濾雜訊
排除僅限內部的變更：
- `chore:`, `ci:`, `build:`, `deps:` — 基礎架構
- `test:`, `spec:` — 僅測試變更
- 內部重構（無面向使用者的影響）

### 4. 為使用者改寫
將技術 commit 訊息轉換為客戶友善的語言：
- 以**好處**為主，而非實作
- 使用**粗體功能名稱**
- 省略程式碼層面的細節
- 修復用過去式，功能用現在式

### 5. 格式化與交付
以結構化 markdown 輸出：
- 版本/日期標題
- 分類區塊
- 面向使用者的說明
- 可選：替代格式（GitHub release、電子郵件、App Store）

## 授權

按現狀提供，用於 changelog 生成工作流程。
