# Google Colab と GitHub を連携した SIGNATE コンペティションワークフロー

## 1. ワークフローの評価

ご提案いただいた「GitHub リポジトリを Google Colab で同期させ、SIGNATE に結果を送信し、変更点を GitHub にコミットして同期させる」というワークフローは、**非常に理にかなっており、推奨される開発スタイルの一つです。**

### メリット
*   **バージョン管理**: 実験の履歴やコードの変更が Git で管理されるため、いつでも過去の状態に戻ったり、変更内容を確認したりできます。
*   **環境の再現性**: 必要なライブラリや設定をリポジトリに含めることで、どの環境（Colab, ローカル PC 等）でも同じ条件で実行できます。
*   **データの安全性**: Google Colab のランタイムがリセットされても、コードは GitHub に、データは Google Drive に保存することで消失を防げます。
*   **自動化**: SIGNATE への提出をコマンドラインから行うことで、手動アップロードの手間とミスを減らせます。

---

## 2. 必要な準備

作業を始める前に、いくつかのアカウントとトークンが必要です。

1.  **GitHub アカウント & リポジトリ**: 作業対象のリポジトリ。
2.  **GitHub Personal Access Token (Classic)**: Colab から GitHub に書き込む（Push する）ために必要です。
    *   [Settings > Developer settings > Personal access tokens > Tokens (classic)](https://github.com/settings/tokens) から作成。
    *   `repo` スコープにチェックを入れてください。
3.  **SIGNATE アカウント & API Token**:
    *   [アカウント設定](https://signate.jp/account_settings) から `signate.json` をダウンロード。
4.  **Google アカウント**: Colab を利用するため。

---

## 3. Google Colab でのセットアップ手順

Google Colab の「シークレット」機能を使うと、パスワードやトークンをコードに直書きせずに安全に扱えます。

### ステップ 1: ノートブックの準備
Colab で新しいノートブックを作成するか、GitHub から既存のノートブックを開きます。

### ステップ 2: シークレットの設定 (推奨)
Colab の左側メニューにある「鍵アイコン (Secrets)」を開き、以下の名前で値を保存します。
*   `GITHUB_TOKEN`: 先ほど取得した GitHub Personal Access Token
*   `USER_EMAIL`: Git のコミットログに使うメールアドレス
*   `USER_NAME`: Git のコミットログに使うユーザー名

### ステップ 3: リポジトリのクローンと環境構築
以下のコードセルを実行して、GitHub からリポジトリをクローンし、必要なライブラリをインストールします。

```python
from google.colab import userdata
import os

# GitHub 認証情報
GITHUB_TOKEN = userdata.get('GITHUB_TOKEN')
USER_EMAIL = userdata.get('USER_EMAIL')
USER_NAME = userdata.get('USER_NAME')
REPO_URL = "https://github.com/USERNAME/REPO_NAME.git" # 自分のリポジトリURLに変更
REPO_NAME = "REPO_NAME"

# Git の設定
!git config --global user.email "$USER_EMAIL"
!git config --global user.name "$USER_NAME"

# クローン (認証付きURLを使用)
if not os.path.exists(REPO_NAME):
  clone_url = REPO_URL.replace("https://", f"https://{USER_NAME}:{GITHUB_TOKEN}@")
  !git clone {clone_url}
  %cd {REPO_NAME}
else:
  %cd {REPO_NAME}
  !git pull

# 依存ライブラリのインストール
!pip install signate
# !pip install -r requirements.txt # もしあれば
```

### ステップ 4: SIGNATE API の設定
`signate.json` をアップロードして API を有効化します。

```python
import os

# フォルダ作成
os.makedirs('/root/.signate', exist_ok=True)

# ここで左のファイルブラウザから signate.json をアップロードするか、
# Google Drive をマウントしてコピーします。
# 例: Google Drive からコピーする場合
# from google.colab import drive
# drive.mount('/content/drive')
# !cp /content/drive/MyDrive/signate.json /root/.signate/

# 権限設定
!chmod 600 /root/.signate/signate.json
```

---

## 4. 開発と提出 (Development Loop)

コードを編集し、学習・推論を行います。結果が出たら SIGNATE に送信します。

```python
# コンペティションのタスクキーとファイルキーを確認
!signate task-list --competition_key=092375ab3c4a43c18c8277e1fd264aa9

# 提出 (例)
!signate submit --task_key=<YOUR_TASK_KEY> ./submission.csv --memo "Colabからの提出"
```

---

## 5. GitHub への同期 (Push)

作業が一段落したら、変更を GitHub に保存します。

```python
# 変更の確認
!git status

# 全ての変更をステージング
!git add .

# コミット
!git commit -m "Update from Google Colab: モデルの改善"

# プッシュ (トークン設定済みなのでパスワード不要)
!git push origin main
```

これで、Colab での作業内容が GitHub に反映されました。
