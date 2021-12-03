# Git

<details open>
<summary>リモート操作</summary>

| 操作 | コマンド |
| - | - |
| リポジトリを取得 | git clone <リポジトリURL> |
| リポジトリをブランチ指定で取得 | git clone –b <ブランチ リポジトリURL> |
| リポジトリをディレクトリ指定で取得 | git clone –b <ブランチ リポジトリURL 取得先ディレクトリ> |
| リポジトリの各ブランチにチェックアウト | git checkout <ブランチ> |
| リモートにローカルブランチを登録 | git push –u origin <登録対象ローカルブランチ> |
| リポジトリのブランチ情報を取得 | git fetch |
| リポジトリのブランチ情報を取得して反映 | git pull |
| リポジトリへ変更を送信 | git push origin <送信先ブランチ> |
| ブランチの削除 | git push –d origin ブランチ ※削除しても他者には見えてしまうため、git fetch –pを入力してもらい、削除の同期をかけてもらうこと |

</details>

<br>

<details>
<summary>ローカル操作 #設定</summary>

| 操作 | コマンド |
| - | - |
| 設定確認 | git config [--system OR global OR local] -l |
| 名前の登録 | git config --global user.name <名前> |
| メールアドレスの登録 | git config --global user.email <メールアドレス> |
| 出力の色付け | git config --global color.ui true |

</details>

<br>

<details open>
<summary>ローカル操作 #セットアップ・管理</summary>

| 操作 | コマンド |
| - | - |
| ローカルリポジトリ初期化 | git init |
| リモートリポジトリと連携 | git remote add origin <リポジトリURL> |
| ワーキングツリー状態確認 | git status |
| 変更差分確認（ワーキングツリー上） | git diff <ファイル> |
| 変更差分確認（ステージング上） | git diff --cached <ファイル> |
| コミットログ確認 | git log [--oneline] |
| 記録の復元（変更取り消し） | git checkout [<コミットID>] <ファイル> |

</details>

<br>

<details>
<summary>ローカル操作 #管理対象確認</summary>

| 操作 | コマンド |
| - | - |
| 管理対象ファイルを一覧表示する | git ls-files |
| 管理対象ファイルを少しずつ表示する | git ls-files \| less |
| 管理対象ディレクトリを一覧表示する* |  |
| 管理対象外ファイルを一覧表示する* |  |
| 非トラッキングファイルを一覧表示する | git untracked |

*管理対象ディレクトリを一覧表示する

```bash
$ git ls-files | sed -e '/^[^\/]*$/d' -e 's/\/[^\/]*$//g' | sort | uniq
```

*管理対象ファイルを一覧表示する
```bash
$ git ls-files --others --exclude-standard

- オプション
  - --others：表示対象を管理対象外ファイルのみにする
  - --exclude-standard：.gitignore等で無視されているファイルを除外する
```

</details>

<br>

<details open>
<summary>ローカル操作 #管理対象管理</summary>

| 操作 | コマンド |
| - | - |
| ファイルを管理対象から除外する（ファイルごと削除） | git rm <ファイル> |
| ファイルを管理対象から除外する（ファイルは削除しない） | git rm --cached <ファイル> |
| ディレクトリを管理対象から除外する（ディレクトリごと削除） | git rm –r <ディレクトリ> |
| ディレクトリを管理対象から除外する（ディレクトリは削除しない） | git rm –r --cached <ディレクトリ> |
| コミットされている.DS_Storeを消す | find . -name .DS_Store -print0 \| xargs -0 git rm -f --ignore-unmatch |

## リモートに登録されているファイルをローカルに残したまま除外する手順
```bash
$ git rm --cached <ファイル>
$ git add -u
$ git commit -m "delete <ファイル>"
$ git push
```

</details>

<br>

<details>
<summary>ローカル操作 #ブランチ操作</summary>

| 操作 | コマンド |
| - | - |
| ブランチの確認 | git branch [-a] |
| ブランチの切り替え | git checkout <ブランチ> |
| ブランチを作成して切り替え | git checkout –b <ブランチ> |
| ブランチの名前変更 | git branch –m 変更前ブランチ名 変更後 ブランチ名 |
| ブランチの削除 | git branch –D ブランチ |
| マージ | git merge <取り込み元ブランチ> ※取り込み先ブランチに切り替えてから実行する |

</details>

<br>

<details open>
<summary>ローカル操作 #追加・追加取消</summary>

| 操作 | コマンド |
| - | - |
| add（ファイルをステージングに追加） | git add <ファイル> |
| 変更を加えたファイルのみステージングに追加 | git add -u |
| addしたファイルをステージングから除外（管理対象からは除外しない） |  |
| commit（リポジトリに履歴を記録） | git commit –m “<コメント>” [-m “<2行目>” -m “<3行目>”] |
| commitの取り消し* |  |
| 直前のコミットメッセージ修正 | git commit --amend -m “修正後メッセージ” |

*addの取り消し
```bash
# ステージ前状態に（作業ディレクトリの変更は保たれる）
$ git reset HEAD <ファイル> 
```

*commitの取り消し
```bash
# ステージ済み状態に戻す（作業ディレクトリの変更は保たれる）
$ git reset --soft HEAD^

# ステージ前状態に（作業ディレクトリの変更は保たれる）
$ git reset HEAD^

# 作業ディレクトリの変更も取り消し
$ git reset --hard HEAD^

# 直前のコミット取り消し（取り消したログを作成）
$ git revert HEAD
```

取り消し系
```bash
# 作業ディレクトリには変更も加えずステージエリアをリセット
$ git reset

# 作業ディレクトリ及びステージエリアをリセット
$ git reset --hard

# <commit>を取り消す為のコミットを作成
$ git revert <commit>

# <commit>まで戻す
$ git reset --hard <commit>
```

</details>

<br>

## git pullの取り消し
1. マージが成功した場合の取り消し
    ```bash
    # マージが成功した場合の取り消し
    $ git reset --hard HEAD@{1}
    ```

2. マージが失敗（コンフリクト）した場合の取り消し（中止）
    ```bash
    # マージが失敗（コンフリクト）した場合の取り消し（中止）
    $ git merge --abort
    ```

<br>

## git pushの取り消し

1. 間違いコミットを打ち消す「revertコミット」を作成して追加pushする  
※推奨
    ```bash
    # 直前のコミットを打ち消すコミット
    $ git revert HEAD

    # 最新を含め、過去4つ分のコミットを打ち消す
    $ git revert HEAD~3

    # pushする
    $ git push origin ブランチ
    ```

2. pushしたコミットを取り消し、リモートから削除する（無かったコトにする）  
※非推奨（この操作をすると、編集していたファイル等が強制削除される）
    ```bash
    # 直前のコミットを取り消す
    $ git reset --hard HEAD^

    # 強制pushする
    $ git push -f origin ブランチ
    ```

<br>

## git mergeの取り消し
1. git revertで打ち消しコミット作成
    ```bash
    # 直前のコミットを打ち消す
    $ git revert -m 1 HEAD

    - オプション
      - m：親番号を指定。1＝マージ先（master）、2＝マージ元（develop）。

    注意）
      revertでマージコミットを打ち消した場合、打ち消されたブランチに含まれていた変更はその後にそのブランチを再度マージしても、取込まれない。
      revertにより変更自体を打ち消しても、マージの事実が歴史上に残り続けるから。
    ```

2. git resetで歴史を強制書き換え
    ```bash
    # 直前のコミットに戻る
    $ git reset --hard HEAD^

    注意）
      --hardオプションはインデックスと作業ツリーも強制的に書き換えるため、保存していない変更は失われる。
    ```

<br>

<details>
<summary>大容量のファイル管理</summary>

- 流れ
  1. {LARGE_FILE}を登録した後、登録済みファイルリストをaddする。
  2. そのあと、{LARGE_FILE}を通常のaddしてcommitすればOK。

| 操作 | コマンド |
| - | - |
| {LARGE_FILE} を登録 | git lfs track {LARGE_FILE} |
| 登録済みファイルリストをadd | git add .gitattributes |

</details>