# GitHub を使ったチーム開発ガイド

このリポジトリでは、メンバー同士が安全かつスムーズに共同開発できるように、基本的な Git/GitHub の使い方をまとめています。

## 1. ブランチを作成して GitHub に公開する

作業を始める前に、リモートリポジトリの最新情報を取得し、作業内容ごとに新しいブランチを作成します。

```bash
# リポジトリを初めて取得する場合
git clone https://github.com/PI11A203-lab/groupc-test.git
cd groupc-test

# 2回目以降は main ブランチを最新にする
git switch main
git pull origin main

# 新しい作業ブランチを作成して移動する
git switch -c feature/login-page

# 作成したブランチを GitHub に公開する
git push -u origin feature/login-page
```

`-u` を付けて最初の push を行うと、次回からは `git push` だけで同じリモートブランチへ送信できます。

## 2. GitHub でできるチームワーク

- **Issue**：バグ、追加機能、担当者、期限などを記録・共有する
- **Branch**：メンバーごとの作業を分離し、互いの変更が衝突しにくくする
- **Pull Request（PR）**：変更内容を説明し、レビューを受けてから `main` に反映する
- **Code Review**：コードへのコメントや修正提案を通じて品質を高める
- **Projects**：タスクの進捗を「未着手・作業中・完了」などに分けて管理する
- **Actions**：テストやビルドを自動実行し、問題を早く発見する

基本的な流れは次のとおりです。

1. Issue を作成して、目的と担当者を決める
2. Issue ごとにブランチを作成する
3. 小さな単位で commit し、GitHub に push する
4. Pull Request を作成する
5. 他のメンバーがレビューする
6. 修正後、承認された Pull Request を `main` にマージする
7. 不要になったブランチを削除する

## 3. commit と push の違い

| 操作 | 保存先 | 役割 |
| --- | --- | --- |
| `git commit` | 自分のパソコン（ローカルリポジトリ） | 変更内容を一つの履歴として記録する |
| `git push` | GitHub（リモートリポジトリ） | ローカルの commit をチームに共有する |

例えば、次の操作では、最初に変更をローカルへ記録し、その後 GitHub へ送信します。

```bash
git add README.md
git commit -m "docs: GitHubの共同作業ガイドを追加"
git push
```

commit しただけでは、変更は GitHub に反映されません。また、push する前に commit が必要です。

## 4. ブランチの分け方

原則として、**一つの目的につき一つのブランチ**を作成します。ブランチ名を見るだけで作業内容が分かる名前にしましょう。

| 種類 | 用途 | 例 |
| --- | --- | --- |
| `feature/` | 新しい機能 | `feature/login-page` |
| `fix/` | バグ修正 | `fix/header-layout` |
| `docs/` | 文書の追加・修正 | `docs/setup-guide` |
| `refactor/` | 動作を変えないコード改善 | `refactor/user-service` |
| `test/` | テストの追加・修正 | `test/login-api` |

大きすぎるブランチはレビューが難しくなるため、短期間で完了できる大きさに分けます。複数人が同じブランチへ直接 push するより、各自がブランチを作って Pull Request を出す方法がおすすめです。

## 5. GitHub に登録・アップロードしてはいけないもの

次の情報は、公開・非公開リポジトリにかかわらず commit しないでください。

- パスワード、API キー、アクセストークン、秘密鍵
- `.env` などの環境変数ファイル
- AWS、Firebase、データベースなどの認証情報
- 個人情報（氏名、住所、電話番号、学生番号など）
- 顧客情報や社外秘の資料
- 著作権やライセンス上、再配布できない画像・音楽・コード
- 容量の大きい生成ファイル、ログ、ビルド成果物
- IDE や OS が自動生成する不要なファイル

`.gitignore` の例：

```gitignore
# 環境変数・秘密情報
.env
.env.*
!.env.example
*.pem
*.key

# 依存パッケージ・ビルド成果物
node_modules/
dist/
build/

# ログ・OS・IDE
*.log
.DS_Store
.idea/
.vscode/
```

`.env.example` には変数名だけを書き、実際の値は入れません。

```dotenv
# 良い例
API_KEY=your_api_key_here
DATABASE_URL=your_database_url_here
```

秘密情報を誤って push した場合、ファイルを削除するだけでは過去の履歴に残ることがあります。直ちにチームへ連絡し、該当するキーやパスワードを無効化・再発行してください。

## 6. 作業前のチェックリスト

- `main` の最新情報から作業ブランチを作成したか
- ブランチ名と commit メッセージから変更内容が分かるか
- パスワードや個人情報が含まれていないか
- 不要なファイルが `.gitignore` に登録されているか
- テストや動作確認を行ったか
- Pull Request に目的、変更内容、確認方法を書いたか

安全な共同開発のため、`main` へ直接 push せず、原則として Pull Request とレビューを通して変更を反映しましょう。
