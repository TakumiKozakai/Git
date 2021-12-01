# Git

<details open>
<summary>リモートリポジトリの操作</summary>

| 操作 | コマンド |
| - | - |
| リモートリポジトリを登録 | git remote add origin <リポジトリURL> |
| リモートリポジトリを取得 | git clone <リポジトリURL> |
| リモートリポジトリをブランチ指定で取得 | git clone –b <ブランチ リポジトリURL> |
| リモートリポジトリをディレクトリ指定で取得 | git clone –b <ブランチ リポジトリURL 取得先ディレクトリ> |
| リモートリポジトリの各ブランチにチェックアウト | git checkout <ブランチ> |
| リモートリポジトリへローカルブランチを登録 | git push –u origin <送信先ブランチ> |
| リモートリポジトリの各ブランチ情報を取得 | git fetch |
| リモートリポジトリの更新を取得して反映 | git pull |
| リモートリポジトリへ変更を送信 | git push origin <送信先ブランチ> |
| リモートブランチの削除 | git push –d origin ブランチ ※削除しても他者には見えてしまうため、git fetch –pを入力してもらい、削除の同期をかけてもらうこと |


</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #設定</summary>

| 操作 | コマンド |
| - | - |
| 設定確認 | git config [--system OR global OR local] -l |
| 名前の登録 | git config --global user.name <名前> |
| メールアドレスの登録 | git config --global user.email <メールアドレス> |
| 出力の色付け | git config --global color.ui true |

</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #管理</summary>

| 操作 | コマンド |
| - | - |
| リポジトリ初期化 | git init |
| 状態確認 | git status |
| ワーキングツリーの変更差分確認 | git diff <ファイル> |
| ステージングの変更差分確認 | git diff --cached <ファイル> |
| コミットログ確認 | git log |
| コミットログ確認（サマリのみ） | git log [--oneline] |
| 記録の復元 | git checkout <コミットID> <ファイル> |

</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #Gitの管理対象の確認</summary>

| 操作 | コマンド |
| - | - |
| 管理対象ファイルを一覧表示する | git ls-files |
| 少しずつ表示する | git ls-files \| less |
| 管理対象外ファイルを一覧表示する | git ls-files --others --exclude-standard |
| トラッキングされていないファイルを一覧表示する | git untracked |

<br>

管理対象ファイルを一覧表示する

```bash
$ git ls-files --others --exclude-standard

- オプション
  - --others：表示対象を管理対象外ファイルのみにする
  - --exclude-standard：.gitignore等で無視されているファイルを除外する
```

<br>

管理対象ディレクトリを一覧表示する

```bash
$ git ls-files | sed -e '/^[^\/]*$/d' -e 's/\/[^\/]*$//g' | sort | uniq
```

</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #Gitの管理対象から除外</summary>

| 操作 | コマンド |
| - | - |
| 管理対象からファイルを除外する（ファイルごと削除） | git rm <ファイル> |
| 管理対象からファイルを除外する（ファイルは削除しない） | git rm --cached <ファイル> |
| 管理対象からディレクトリを除外する（ディレクトリごと削除） | git rm –r <ディレクトリ> |
| 管理対象からディレクトリを除外する（ディレクトリは削除しない） | git rm –r --cached <ディレクトリ> |
| コミットされちゃってる.DS_Storeを消す | find . -name .DS_Store -print0 \| xargs -0 git rm -f --ignore-unmatch |

</details>

<br>

## git addの取り消し
1.リモートに登録されているファイルを除外する（ローカルには残したまま）
```
$ git rm --cached <ファイル>

$ git add -u

$ git commit -m "delete <ファイル>"

$ git push
```

<br>

<details open>
<summary>ローカルリポジトリの操作 #ブランチ操作</summary>

| 操作 | コマンド |
| - | - |
| ブランチの確認 | git branch [-a] |
| ブランチの切り替え | git checkout <ブランチ> |
| ブランチを作成して切り替え | git checkout –b <ブランチ> |
| ローカルブランチの名前変更 | git branch –m 変更前ブランチ名 変更後 ブランチ名 |
| ローカルブランチの削除 | git branch –D ブランチ |
| マージ | git merge <取り込み元ブランチ> ※取り込み先ブランチに切り替えてから実行する |

</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #変更取消</summary>

| 操作 | コマンド |
| - | - |
| ローカルの変更取消 | git checkout . |

</details>

<br>

<details open>
<summary>ローカルリポジトリの操作 #追加・追加取消</summary>

| 操作 | コマンド |
| - | - |
| ファイルをインデックスに追加 | git add <ファイル> |
| 変更を加えたファイルのみインデックスに追加 | git add -u |
| addしたファイルの除外（管理対象からは除外しない） | git reset HEAD <ファイル> |
| リポジトリに履歴を記録 | git commit –m “<コメント>” [-m “<2行目>” -m “<3行目>”] |
| 直前のコミットメッセージ修正 | git commit --amend -m “修正後メッセージ” |

</details>

<br>

## git pullの取り消し
1.マージが成功した場合の取り消し
```
$ git reset --hard HEAD@{1}
```

<br>

2.マージが失敗（コンフリクト）した場合の取り消し（中止）
```
$ git merge --abort
```

<br>

## git pushの取り消し

1.間違いコミットを打ち消す「revertコミット」を作成して追加pushする  
※推奨
```
# 直前のコミットを打ち消すコミット
$ git revert HEAD

# 最新を含め、過去4つ分のコミットを打ち消す
$ git revert HEAD~3

# pushする
$ git push origin ブランチ
```

<br>

2.pushしたコミットを取り消し、リモートから削除する（無かったコトにする）  
※非推奨  
この操作をすると、編集していたファイル等が強制削除される

```
# 直前のコミットを取り消す
$ git reset --hard HEAD^

# 強制pushする
$ git push -f origin ブランチ
```

<br>

## git mergeの取り消し
### １．git revertで打ち消しコミット作成
```
# 直前のコミットを打ち消す
$ git revert -m 1 HEAD

- オプション
  - m：親番号を指定。1＝マージ先（master）、2＝マージ元（develop）。

注意）
  revertでマージコミットを打ち消した場合、打ち消されたブランチに含まれていた変更は、
  その後、そのブランチを再度マージしても、取込まれない。
  revertにより変更自体を打ち消しても、マージの事実が歴史上に残り続けるから。
```



### ２．git resetで歴史を強制書き換え
```
# 直前のコミットに戻る
$ git reset --hard HEAD^

注意）
  --hardオプションはインデックスと作業ツリーも強制的に書き換えるため、保存していない変更は失われる。
```


<br>

<details open>
<summary>大容量のファイル管理</summary>

流れ
1. {LARGE_FILE]を登録した後、登録済みをファイルリストをaddする。
2. そのあと、LARGE_FILEを通常のaddしてcommitすればOK。

| 操作 | コマンド |
| - | - |
| {LARGE_FILE} を登録 | git lfs track {LARGE_FILE} |
| 登録済みファイルリストをadd | git add .gitattributes |

</details>

<br>

| 操作 | コマンド |
| - | - |
|  |  |
|  |  |
|  |  |