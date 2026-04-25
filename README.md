# signate_practice_bank

SIGNATEのコンペティション（練習問題）に取り組むためのプロジェクトリポジトリです。

## プロジェクトの進め方

Google ColabとGitHubを活用した開発ワークフローは以下のようになります。

### ⚠️ 注意：自動同期はされません！
「Google Colab上で編集しても、GitHub上で編集しても勝手に同期される」**わけではありません**。
Google Colabはあくまで一時的な実行環境（仮想マシン）です。そのため、Colab上で作成・編集したコードやノートブックをGitHubのこのリポジトリに反映させるためには、**手動でコミットとプッシュ（保存と送信）**を行う必要があります。

### プロジェクト進行のステップ

プロジェクトは基本的に以下のサイクルで進めていきます。

1. **環境のセットアップ**: Google Colab上でこのリポジトリをクローン（ダウンロード）し、SIGNATE APIの準備をします。
2. **データのダウンロード**: SIGNATE APIを使ってコンペティションのデータをダウンロードします。
3. **データ分析・モデリング**: ノートブック上でコードを書き、データ処理やAIモデルの学習を行います。
4. **結果の提出**: 作成した予測ファイル（CSVなど）を、SIGNATE APIを使って提出します。
5. **GitHubへの保存 (重要)**: 区切りが良いところで、作業内容をGitHubにプッシュ（アップロード）して保存します。

詳細な手順については [COLAB_WORKFLOW.md](./COLAB_WORKFLOW.md) にまとまっています。

---

## 新しく作成したGoogle Colabファイルとリポジトリを紐づける方法

すでにGoogle Drive上で新しいColabファイルを作成し、そこでSIGNATEのセットアップを行った場合、以下の2つの方法でこのリポジトリに紐づける（保存する）ことができます。

### 方法1: Colabのメニューから直接GitHubに保存する（最も簡単）
作成したノートブックファイル（`.ipynb`）をとりあえずGitHubに保存したい場合は、以下の手順が簡単です。
1. Google Colabの画面左上のメニューから `ファイル` > `GitHub にコピーを保存` をクリックします。
2. GitHubの認証を求められたら承認します。
3. リポジトリの選択欄で `Ryutta/signate_practice_bank` を選びます。
4. ブランチ（通常は `main`）と、保存するファイルパス（例: `notebooks/my_practice.ipynb`）を指定します。
5. コミットメッセージ（「最初のノートブックを追加」など）を入力して「OK」を押すと、GitHubにノートブックがアップロードされます。

### 方法2: Colab内でGitコマンドを使って管理する（推奨）
本格的にプロジェクトを進めていくなら、Colab環境にこのリポジトリを丸ごとコピーして作業し、Gitコマンドで保存する方法がおすすめです。
すでに用意されているテンプレート [notebooks/Signate_Workflow_Template.ipynb](./notebooks/Signate_Workflow_Template.ipynb) を活用します。

1. ご自身が作成したノートブックのコードをコピーするか、あるいは用意された `Signate_Workflow_Template.ipynb` をGoogle Colabで開いて作業のベースにします。
2. テンプレート内の「1. 環境設定とリポジトリのクローン」のセルを実行します。これでColab環境内に `signate_practice_bank` フォルダが作成され、リポジトリと紐づきます。
3. 作業（コードの編集など）が終わったら、テンプレートの下部にある「7. GitHubへの同期 (Push)」のセル（`!git add .` や `!git commit`, `!git push`）を実行することで、変更内容がこのリポジトリに同期（保存）されます。

## その他参考資料
* SIGNATE APIの設定やコマンドの使い方: [SIGNATE_SETUP.md](./SIGNATE_SETUP.md)
* 開発ワークフローの詳細: [COLAB_WORKFLOW.md](./COLAB_WORKFLOW.md)
