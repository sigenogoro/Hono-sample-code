# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 開発環境

VS Code Dev Containers を使用したリモートコンテナ環境で開発を行います。

- **ベースイメージ**: `mcr.microsoft.com/devcontainers/base:ubuntu`
- **Node.js**: v24（devcontainer feature 経由でインストール）
- **コンテナ設定**: `.devcontainer/devcontainer.json`

コンテナ起動方法: VS Code のコマンドパレットから `Dev Containers: Reopen in Container` を実行する。

## プロジェクト構成

アプリケーションコードはすべて `my-app/` 配下にあります。コマンドの実行は基本的に `my-app/` ディレクトリで行います。

```
my-app/
  src/index.ts   # エントリーポイント — Honoアプリ定義とサーバー起動
  dist/          # TypeScriptコンパイル出力（NodeNext ESM形式）
  package.json
  tsconfig.json
```

## コマンド

`my-app/` ディレクトリで実行：

```bash
npm run dev          # ホットリロード付き開発サーバー起動（tsx watch、ポート3000）
npm run build        # TypeScript → dist/ へコンパイル
npm start            # コンパイル済み成果物を実行
npm test             # テスト実行（Vitest）
npm run test:watch   # テストをウォッチモードで実行
npm run lint         # ESLint実行
npm run format       # Prettierでフォーマット
```

## アーキテクチャ

サーバーは `@hono/node-server` アダプター上で [Hono](https://hono.dev/) を使用しています。基本的なパターンは以下の通りです：

1. `new Hono()` でアプリインスタンスを作成（サブアプリも同様）
2. アプリインスタンスにルートを登録
3. `@hono/node-server` の `serve()` に `app.fetch` を渡してサーバーを起動

## REST API開発のパターン

**ルートの分割** — `app.route()` でサブアプリをマウントし、`src/index.ts` を整理する：

```ts
// src/routes/users.ts
import { Hono } from 'hono'
const users = new Hono()
users.get('/', (c) => c.json([]))
export default users

// src/index.ts
import users from './routes/users.js'
app.route('/users', users)
```

**レスポンスヘルパー** — コンテキストオブジェクト `c` の `c.json()`、`c.text()`、`c.status()` などを使用します。

**ミドルウェア** — `app.use()` でアプリ全体またはパス単位に適用できます。Hono組み込みのCORS・ロガー・認証などが利用可能：

```ts
import { cors } from 'hono/cors'
import { logger } from 'hono/logger'
app.use('*', logger())
app.use('/api/*', cors())
```

**importのパス注意点** — `tsconfig.json` で `"module": "NodeNext"` と `"verbatimModuleSyntax": true` が設定されているため、ローカルモジュールのimportには `.js` 拡張子が必要です（コンパイル後のパスを指定）：

```ts
import { foo } from './foo.js'
```