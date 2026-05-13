# 編集部掲示板（幻冬社ルネッサンス編集局サイト）

社内編集業務ポータル。GitHub Pages で公開し、URL を知るメンバーが閲覧可能。

## 公開アーキテクチャ

```
[作成者] ─編集→ default-data.js / index.html
                    │ git push
                    ▼
              [GitHub リポジトリ]
                    │ 自動デプロイ
                    ▼
              [GitHub Pages] ─URL─→ 閲覧者
```

## 構成ファイル

| ファイル | 役割 |
|---|---|
| `index.html` | サイト本体（React + Tailwind、単一HTML） |
| `default-data.js` | 全コンテンツ（ナビ・お知らせ・各ページ） |
| `.gitignore` | 除外設定 |

## 更新の流れ

1. ローカルで内容を編集（直接ファイル編集／ブラウザ管理者UIで編集 → JSONエクスポート→ Claude Code に取り込み）
2. `git add -A && git commit -m "更新内容"`
3. `git push`
4. 数十秒〜2分で GitHub Pages に自動反映、閲覧者は次回リロードで最新表示

## GitHub Pages へのデプロイ手順（初回）

### 1. GitHub アカウントとリポジトリの準備

[github.com](https://github.com) でアカウント作成後、新規リポジトリを作成（例：`henshukyoku-portal`）。**Public** にすると URL を知る誰でも閲覧可能。

### 2. リポジトリと接続

PowerShell でこのフォルダに入り、自分の情報をセット：

```powershell
git config user.name "あなたの名前"
git config user.email "you@example.com"
git remote add origin https://github.com/YOUR_USERNAME/henshukyoku-portal.git
git branch -M main
git push -u origin main
```

初回 push 時に GitHub の認証が求められます。

### 3. GitHub Pages を有効化

リポジトリの **Settings → Pages** で、Source を `Deploy from a branch`、ブランチを `main` / `(root)` に設定 → Save。

数十秒後、`https://YOUR_USERNAME.github.io/henshukyoku-portal/` で閲覧可能になります。

## 注意事項

- 管理者パスワードはクライアントJSに含まれます。**公開後のブラウザ管理者UIはあくまでローカル編集プレビュー用**で、その編集は自端末の localStorage にしか保存されません。本番への反映は必ず作成者がファイル編集→ push で行ってください。
- `default-data.js` には画像が base64 で埋め込まれており約 4.8MB あります。初回読み込みに数秒かかります。
