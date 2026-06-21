# Hono 開発

TypeScript + Hono + Node.js で REST API を開発する学習プロジェクト。

## 技術スタック

- **Runtime**: Node.js v24
- **Framework**: Hono + @hono/node-server
- **Language**: TypeScript（strict モード）
- **Linter**: ESLint
- **Formatter**: Prettier
- **開発環境**: VS Code Dev Containers（Ubuntu ベース）

## 環境構築

### 前提条件

- Docker
- VS Code
- VS Code 拡張機能「Dev Containers」（`ms-vscode-remote.remote-containers`）

### 起動手順

1. リポジトリをクローン

```bash
git clone https://github.com/<your-name>/<your-repo>.git
cd <your-repo>
```

2. VS Code でフォルダを開く

```bash
code .
```

3. コマンドパレットから `Dev Containers: Reopen in Container` を実行

4. コンテナ起動後、`my-app/` に移動して依存関係をインストール

```bash
cd my-app
npm install
```

## コマンド

`my-app/` ディレクトリで実行：

```bash
npm run dev          # 開発サーバー起動（ポート 3000）
npm run build        # TypeScript → dist/ へコンパイル
npm start            # コンパイル済み成果物を実行
npm test             # テスト実行
npm run lint         # ESLint 実行
npm run format       # Prettier でフォーマット
```

## ディレクトリ構成

```
.
├── .devcontainer/
│   └── devcontainer.json  # Dev Containers 設定
├── my-app/
│   ├── src/
│   │   ├── index.ts       # エントリーポイント
│   │   └── routes/        # ルーティング定義
│   ├── dist/              # コンパイル出力（Git 管理外）
│   ├── package.json
│   └── tsconfig.json
├── .gitignore
├── CLAUDE.md
└── README.md
```

## Claude Code
コンテナ内で Claude Code が使える。初回のみ認証が必要。
```bash
claude
```
2回目以降は認証不要（volume に認証情報を保持）。