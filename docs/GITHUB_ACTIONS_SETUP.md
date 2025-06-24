# GitHub Actions セットアップガイド

## sync-upstream.yml ワークフローの設定

このワークフローを正常に動作させるには、以下のいずれかの設定が必要です：

### 方法1: リポジトリの権限設定（推奨）

1. GitHubリポジトリの **Settings** → **Actions** → **General** に移動
2. **Workflow permissions** セクションで以下を設定：
   - **Read and write permissions** を選択
   - **Allow GitHub Actions to create and approve pull requests** にチェック

### 方法2: Personal Access Token (PAT) を使用

1. GitHub個人設定から[Personal Access Token](https://github.com/settings/tokens/new)を作成
   - 必要な権限: `repo`, `workflow`
2. リポジトリの **Settings** → **Secrets and variables** → **Actions** で以下を設定：
   - Name: `PAT_TOKEN`
   - Value: 作成したトークン

### 方法3: GitHub App を使用（高度な設定）

1. [GitHub App](https://github.com/settings/apps/new)を作成
2. 必要な権限を設定
3. リポジトリの **Settings** → **Secrets and variables** → **Actions** で以下を設定：
   - `APP_ID`: GitHub AppのID
   - `APP_PRIVATE_KEY`: GitHub Appの秘密鍵

## トラブルシューティング

### "GitHub Actions is not permitted to create or approve pull requests" エラー

このエラーが表示される場合は、上記の方法1の設定を確認してください。

### ラベルが見つからないエラー

ワークフローは必要なラベルを自動的に作成しますが、権限不足で作成できない場合があります。
手動でラベルを作成する場合は、以下のラベルを追加してください：

- `upstream-sync` (color: #0366d6)
- `automated` (color: #f9d0c4)
- `needs-review` (color: #d93f0b)
- `needs-attention` (color: #e11d21)