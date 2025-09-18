# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a TypeScript project that integrates with Ableton Live through the Model Context Protocol (MCP). The project contains:
- TypeScript configuration for building a Node.js application
- An embedded Python-based Ableton MCP server (`ableton-mcp/`) that enables Claude AI to control Ableton Live

## Development Commands

### TypeScript/Node.js Development
```bash
# Install dependencies
npm install

# Run in development mode
npm run dev

# Build the project
npm run build

# Start production build
npm run start

# Type checking
npm run typecheck

# Linting
npm run lint
npm run lint:fix

# Code formatting
npm run format

# Testing
npm run test
npm run test:watch  # Watch mode
npm run test:coverage  # With coverage
```

### Ableton MCP Server
The `ableton-mcp` subdirectory contains a Python-based MCP server that connects to Ableton Live:
- Entry point: `MCP_Server/server.py`
- Remote Script: `AbletonMCP_Remote_Script/__init__.py` (must be installed in Ableton)
- Managed with `uv` package manager
- Run via: `uvx ableton-mcp`

## Architecture

### Communication Flow
1. **Ableton Live** ← Remote Script (`AbletonMCP_Remote_Script/__init__.py`) creates socket server
2. **MCP Server** (`MCP_Server/server.py`) connects to Remote Script via TCP socket
3. **Claude AI** interacts with MCP Server through Model Context Protocol

### Key Components
- **Socket Communication**: JSON-based protocol over TCP (port defined in Remote Script)
- **MCP Implementation**: Uses FastMCP framework for implementing Model Context Protocol
- **Remote Script**: Runs inside Ableton Live, executes commands received via socket

## Testing Approach
- Jest is configured for TypeScript testing
- Test files should use `.test.ts` or `.spec.ts` extensions
- Run single test: `npm test -- path/to/test.ts`

## Important Notes
- The project requires both Node.js/TypeScript setup and Python/Ableton configuration
- Ableton Remote Script must be installed in Ableton's MIDI Remote Scripts directory
- Only one MCP server instance should run at a time (either Claude Desktop or Cursor)

## Ableton Music Production Workflow

### ハイブリッドアプローチ（Session → Arrangement）
1. **Session Viewで各セクションのパーツを作る**
   - 実験的にループを作成
   - 縦にバリエーション、横にセクションで整理

2. **Scene（横列）として整理**
   - Scene 1: イントロ
   - Scene 2: Aメロ（Verse）
   - Scene 3: Bメロ（Pre-Chorus）
   - Scene 4: サビ（Chorus）
   - Scene 5: ブリッジ
   - Scene 6: アウトロ

3. **満足したらArrangement Viewへ録音**
   - Session ViewからRecord
   - シーンを順番にトリガー

4. **Arrangement Viewで細かい調整**
   - オートメーション描画
   - トランジション調整
   - 最終的なミックス

### 作業のポイント
- Session Viewは**アイデア出し**と**実験**に最適
- Arrangement Viewは**完成形の構築**と**細部調整**に最適
- 両方の利点を活かすハイブリッドワークフローが効率的

## プロジェクト記録方針

### リアルタイム記録なしアプローチ
1. **Abletonで自由に実験**
   - 創作の流れを最優先
   - 記録のことは気にせず制作に集中

2. **区切りの良いタイミングで保存**
   - 「このセッション保存して」と指示
   - 一括で全トラック・クリップ情報を取得
   - JSONファイルとMarkdownで記録

3. **保存タイミングの目安**
   - 各セクション（Aメロ、サビ等）完成時
   - 作業セッション終了時
   - 満足いくバージョンができた時

### メリット
- 創作の流れを妨げない
- 試行錯誤を自由にできる
- 必要な時だけ効率的に記録
- 後から完全に復元可能