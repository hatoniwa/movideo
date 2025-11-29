# Termuxを使ったファイルアップロードガイド

このガイドでは、Android端末のダウンロードフォルダ（`~/storage/downloads`）にあるファイルを、Termuxを使ってGitHubリポジトリにアップロードする方法を説明します。

---

## 📋 目次

1. [Termuxのインストールと初期設定](#1-termuxのインストールと初期設定)
2. [ストレージへのアクセス権限を取得](#2-ストレージへのアクセス権限を取得)
3. [Gitのインストールと設定](#3-gitのインストールと設定)
4. [リポジトリのクローン](#4-リポジトリのクローン)
5. [ダウンロードフォルダからファイルをコピー](#5-ダウンロードフォルダからファイルをコピー)
6. [GitHubへプッシュ](#6-githubへプッシュ)
7. [便利なTips](#7-便利なtips)

---

## 1. Termuxのインストールと初期設定

### Termuxをインストール

1. [F-Droid](https://f-droid.org/packages/com.termux/) からTermuxをインストールします
   - ※Google Playストア版は更新が止まっているため、F-Droid版を推奨

2. Termuxを起動し、パッケージを更新します：

```bash
pkg update && pkg upgrade
```

---

## 2. ストレージへのアクセス権限を取得

Termuxから端末のストレージ（ダウンロードフォルダなど）にアクセスするには、権限設定が必要です。

```bash
termux-setup-storage
```

このコマンドを実行すると、ストレージへのアクセス許可を求めるダイアログが表示されます。**「許可」**をタップしてください。

成功すると、以下のようなシンボリックリンクが作成されます：

```
~/storage/
├── downloads   → ダウンロードフォルダ
├── dcim        → カメラで撮影した写真・動画
├── pictures    → 画像フォルダ
├── music       → 音楽フォルダ
├── movies      → 動画フォルダ
└── shared      → 内部ストレージのルート
```

### ダウンロードフォルダの内容を確認

```bash
ls ~/storage/downloads
```

---

## 3. Gitのインストールと設定

### Gitをインストール

```bash
pkg install git
```

### ユーザー情報を設定

```bash
git config --global user.name "あなたの名前"
git config --global user.email "your.email@example.com"
```

### GitHub認証の設定

GitHubにプッシュするには認証が必要です。Personal Access Token (PAT)を使用する方法が推奨されます。

1. GitHubで [Personal Access Token](https://github.com/settings/tokens) を作成
2. 必要なスコープ: `repo`（フルアクセス）

---

## 4. リポジトリのクローン

### HTTPSでクローン

```bash
# ホームディレクトリに移動
cd ~

# リポジトリをクローン
git clone https://github.com/hatoniwa/movideo.git

# リポジトリに移動
cd movideo
```

**初回プッシュ時に認証情報を入力します：**
- Username: あなたのGitHubユーザー名
- Password: Personal Access Token（パスワードではありません）

### 認証情報を保存（オプション）

毎回入力するのが面倒な場合は、認証情報を保存できます：

```bash
git config --global credential.helper store
```

※セキュリティ上の理由から、個人の端末でのみ使用してください。

---

## 5. ダウンロードフォルダからファイルをコピー

### 特定のファイルをコピー

```bash
# 例: ダウンロードフォルダからmp4ファイルをコピー
cp ~/storage/downloads/my_video.mp4 ~/movideo/docs/
```

### 複数のファイルをコピー

```bash
# 例: すべてのjpgファイルをコピー
cp ~/storage/downloads/*.jpg ~/movideo/docs/

# 例: 特定のパターンに一致するファイルをコピー
cp ~/storage/downloads/video_*.mp4 ~/movideo/docs/
```

### ディレクトリごとコピー

```bash
# -r オプションでディレクトリを再帰的にコピー
cp -r ~/storage/downloads/my_folder ~/movideo/docs/
```

### ファイルを確認

```bash
ls -la ~/movideo/docs/
```

---

## 6. GitHubへプッシュ

### 変更を確認

```bash
cd ~/movideo
git status
```

### ファイルをステージング

```bash
# 特定のファイルを追加
git add docs/my_video.mp4

# または、すべての変更を追加
git add .
```

### コミット

```bash
git commit -m "Add new video file from downloads"
```

### プッシュ

```bash
git push origin main
```

---

## 7. 便利なTips

### ファイル名に日本語が含まれる場合

```bash
# 日本語ファイル名を正しく表示
git config --global core.quotepath false
```

### 大きなファイルを扱う場合

大きなファイル（100MB以上）をプッシュする場合は、Git LFSの使用を検討してください：

```bash
pkg install git-lfs
git lfs install
git lfs track "*.mp4"
git add .gitattributes
```

### よく使うコマンドのエイリアス

```bash
# .bashrcに追加
echo "alias dl='ls ~/storage/downloads'" >> ~/.bashrc
echo "alias mvd='cd ~/movideo'" >> ~/.bashrc
source ~/.bashrc
```

### 作業の流れまとめ

```bash
# 1. ダウンロードフォルダを確認
ls ~/storage/downloads

# 2. リポジトリに移動
cd ~/movideo

# 3. 最新の状態に更新
git pull

# 4. ファイルをコピー
cp ~/storage/downloads/new_file.mp4 ./docs/

# 5. 変更をコミット＆プッシュ
git add .
git commit -m "Add new file"
git push
```

---

## ❓ トラブルシューティング

### `Permission denied` エラーが出る場合

```bash
# ストレージ権限を再設定
termux-setup-storage
```

### `fatal: not a git repository` エラーが出る場合

```bash
# リポジトリのディレクトリにいることを確認
cd ~/movideo
pwd
```

### プッシュ時に認証エラーが出る場合

- Personal Access Tokenが正しいか確認
- トークンに `repo` スコープが含まれているか確認
- トークンが期限切れでないか確認

---

## 📚 参考リンク

- [Termux Wiki](https://wiki.termux.com/wiki/Main_Page)
- [GitHub Personal Access Token の作成](https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- [Git 公式ドキュメント](https://git-scm.com/doc)

---

> このガイドについて質問がある場合は、Issueを作成してください。
