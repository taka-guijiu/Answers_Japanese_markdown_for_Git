## Chapter 14

### 1. この課題の意図は、14.2.2項のようにgit mergeを使うことです。

```
% git merge FETCH_HEAD
Auto-merging another_rename
CONFLICT (content): Merge conflict in another_rename
Automatic merge failed; fix conflicts and then commit the result.
＜和訳＞
another_rename の自動マージ
CONFLICT (内容)： another_renameでコンフリクトが発生しました。
自動マージに失敗しました。コンフリクトを修正してから結果をコミットしてください。
```
14.3.4項と同じようなコンフリクトがあることに気づくだろう。another_renameファイルはlist14.17のようになる。
```
% cat another_rename
Small change
Tiny change
Small change 2
ABC DEF GHI JKL MNO PQR
ABC
<<<<<<< HEAD
DEF
=======
ABC
>>>>>>> fe8b543387bdc3aa87e4a93cb5cc91755a3f86fd
```

この時点で、ファイルを編集してコンフリクトを取り除き、git addとgit commitを実行してください。

```
% git add another_rename
```
```
% git commit
[master f243f91] Merge branch 'master' of /home/rick/work/math
＜和訳＞
[master f243f91] /home/rick/work/math の 'master' ブランチをマージする。
```
git commitを実行すると、デフォルトのメッセージがエディタに表示されます。このメッセージを編集するか受け入れるかして、コミットを完了させましょう。それから、すべてのリポジトリを同期させるために (そしてこの章の練習問題の残りの項目のために)、git push を実行します。
```
% git push
Counting objects: 6, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 582 bytes | 291.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
To /home/rick/work/math.git
fe8b543..f243f91 master -> master
＜和訳＞
オブジェクトを数える： 6, 完了。
最大2スレッドを使用するデルタ圧縮。
オブジェクトを圧縮： 100% (6/6)、完了。
オブジェクトの書き込み： 100% (6/6), 582 bytes | 291.00 KiB/s, 完了。
合計 6 (デルタ 4)、再利用 0 (デルタ 0)
/HOME/rick/work/math.git に対して
fe8b543..f243f91 master -> master
```

アンサーガイドのこの章の残りの部分は、リポジトリが同期していることを前提としている。

### 2. math.billリポジトリの各ブランチにコミットを追加する手順は以下のようになる：

```
% cd $HOME/math.bill
```
```
% git branch
* master
```
```
% git branch --all
* master
remotes/origin/HEAD -> origin/master
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature
```

他のブランチを見るには、git branch で --all スイッチを使う必要があることに注意しましょう。another_fix_branch ブランチと new_feature ブランチは、現時点では単なるリモート追跡ブランチです。これらのブランチには、git checkout を使ってローカルにアクセスします。

```
% git checkout another_fix_branch
Branch 'another_fix_branch' set up to track remote branch \ 'another_fix_branch' from 'origin'.
Switched to a new branch 'another_fix_branch'
＜和訳＞
'another_fix_branch' ブランチが 'origin' からのリモートブランチ 'another_fix_branch' を追跡するように設定されました。
新しいブランチ 'another_fix_branch' に切り替わりました。
```
このブランチへのコミットをチェックインするには、次のようにする：

```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme"
[another_fix_branch 8b50a49] small change to readme
1 file changed, 1 insertion(+)
＜和訳＞
[another_fix_branch 8b50a49] readme の小さな変更
1 ファイル変更、1 挿入(+)
```

次に、new_featureブランチにも同じことをする。

```
% git checkout new_feature
Branch 'new_feature' set up to track remote branch 'new_feature' from 'origin'.
Switched to a new branch 'new_feature'
＜和訳＞
ブランチ 'new_feature' は、'origin' からのリモートブランチ 'new_feature' を追跡するように設定されました。
新しいブランチ 'new_feature' に切り替わった
```
```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme"
[new_feature 4a0ef84] small change to readme
1 file changed, 1 insertion(+)
＜和訳＞
[new_feature 4a0ef84] readmeの小さな変更
1 ファイル変更、1 挿入(+)
```

次に、masterブランチにも同じ変更を加えます。

```
% git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
＜和訳＞
ブランチを 'master' に切り替えました。
ブランチが 'origin/master' に更新されました。
```
```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme"
[master 135d398] small change to readme
1 file changed, 1 insertion(+)
＜和訳＞
[master 135d398] readmeの小さな変更
1 ファイル変更、1 挿入(+)
```

この時点で、math.bill のすべてのブランチに小さな変更が加わっています。git push --all を使って、これらのブランチをすべて math.git にプッシュしてみましょう。このように表示されるはずです：

```
% git push --all
Counting objects: 39, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (34/34), done.
Writing objects: 100% (39/39), 3.38 KiB | 866.00 KiB/s, done.
Total 39 (delta 18), reused 0 (delta 0)
To /home/rick/work/math.git
eca99b1..8b50a49 another_fix_branch -> another_fix_branch
158f783..4a0ef84 new_feature -> new_feature
! [rejected] master -> master (fetch first)
error: failed to push some refs to '/home/rick/work/math.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
＜和訳＞
オブジェクトを数える： 39個、完了。
最大2スレッドを使用するデルタ圧縮。
オブジェクトを圧縮： 100% (34/34)、完了。
オブジェクトの書き込み： 100% (39/39), 3.38 KiB | 866.00 KiB/s, 完了。
合計39 (デルタ18)、再利用0 (デルタ0)
ホーム/rick/work/math.git へ
eca99b1..8b50a49 another_fix_branch -> another_fix_branch です。
158f783..4a0ef84 new_feature -> new_feature
! [reject] master -> master (フェッチが先)
error: '/home/rick/work/math.git' への参照プッシュに失敗しました。
hint: リモートにローカルにない作業が含まれていたため、
hint: 更新が拒否されました。
hint: これは通常、別のリポジトリが同じ参照にプッシュしていることが原因です。
hint: 再度プッシュする前に、まずリモートの変更を統合（'git pull ...' など）すると
hint: よいでしょう。
hint: 詳細は 'git push --help' の 'ファストフォワードに関するhint: 注意' を参照してください。
```

前の練習で行った git push のせいで、master を math.git にプッシュすることができません。math.git の master のほうが新しい変更があるからです。しかし、他のブランチは math.git にプッシュされています。このことは、いくつかの方法で証明することができます。最初の方法は git remote -v show origin です。

```
% git remote -v show origin
* remote origin
Fetch URL: /home/rick/work/math.git
Push URL: /home/rick/work/math.git
HEAD branch: master
Remote branches:
another_fix_branch tracked
master tracked
new_feature tracked
Local branches configured for 'git pull':
another_fix_branch merges with remote another_fix_branch
master merges with remote master
new_feature merges with remote new_feature
Local refs configured for 'git push':
another_fix_branch pushes to another_fix_branch (up to date)
master pushes to master (local out of date)
new_feature pushes to new_feature (up to date)
＜和訳＞
* remote origin
Fetch URL: /home/rick/work/math.git
Push URL: /home/rick/work/math.git
HEAD branch: master
Remote branches:
another_fix_branch が追跡されました。
マスターが追跡された
new_featureが追跡された
git pull」用に設定されたローカルブランチ：
another_fix_branch はリモートの another_fix_branch とマージします。
master はリモートの master とマージします
new_feature はリモートの new_feature とマージします。
git push」用に設定されたローカル参照：
another_fix_branch は another_fix_branch にプッシュします (最新)
master は master にプッシュします (ローカルは最新ではありません)
new_feature は new_feature にプッシュします (最新)
```

出力は、masterだけが古いが、他の2つは最新であることを示しています。math.git に私たちの変更があることを証明するもうひとつの方法は、git ls-remote を使うことです。まず math.bill で行ったコミットの SHA1 ID を取得し、それから git ls-remote を呼び出します：

```
% git log --all --oneline -n 3
135d398 (HEAD -> master) small change to readme
4a0ef84 (origin/new_feature, new_feature) small change to readme
8b50a49 (origin/another_fix_branch, another_fix_branch) small change to readme
＜和訳＞
135d398 (HEAD -> master) readme の小さな変更。
4a0ef84 (origin/new_feature, new_feature) readme への小さな変更。
8b50a49 (origin/another_fix_branch, another_fix_branch) readme に小さな変更。
```
```
% git ls-remote
From /home/rick/work/math.git
f243f91c5eaed9f4ef076218a23768720ce484a0 HEAD
8b50a49dfeb431660d1adcd883e01ca2f22281ab refs/heads/another_fix_branch
f243f91c5eaed9f4ef076218a23768720ce484a0 refs/heads/master
4a0ef848e2fd71d9579c7765fcb6224ba93b23d3 refs/heads/new_feature
1b24001489ed2ebab2afc5097228c92b771e7141 refs/tags/four_files_galore
95f466d43bc082ea18b29ad3a2436ce342237d86 refs/tags/four_files_galore^{}
```

another_fix_branch と new_feature の SHA1 ID は math.bill と math.git で同じです。masterのSHA1 IDは同じではありません（'out of date'）。math.carol に移動して git フェッチすると、ブランチの SHA1 ID が更新されて math.git のものと一致することがわかります。

```
% cd $HOME/math.carol
```
```
% git log --all --decorate --oneline
f243f91 (HEAD -> master, origin/master, origin/HEAD) Merge branch 'master' \ of /home/rick/work/math
... log lines omitted ...
158f783 (origin/new_feature) Starting a second new file
... log lines omitted ...
eca99b1 (origin/another_fix_branch) Renaming c and d.
... log lines omitted ...
＜和訳＞
f243f91 (HEAD -> master, origin/master, origin/HEAD) /home/rick/work/math のブランチ 'master' をマージした。
... ログ行省略 ...
158f783 (origin/new_feature) 2番目の新しいファイルを開始します。
... ログ行省略 ...
eca99b1 (origin/another_fix_branch) c と d の名前を変更。
... ログ行省略 ...
```
```
% git fetch
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
From /home/rick/work/math
eca99b1..8b50a49 another_fix_branch -> origin/another_fix_branch
158f783..4a0ef84 new_feature -> origin/new_feature
＜和訳＞
リモート： オブジェクトを数えています： 5, 完了
リモート： オブジェクトを圧縮： 100% (4/4), 完了。
リモート： 合計5 (デルタ2)、再利用0 (デルタ0)
オブジェクトを展開： 100% (5/5)、完了。
home/rick/work/mathより
eca99b1..8b50a49 another_fix_branch -> origin/another_fix_branch
158f783..4a0ef84 new_feature -> origin/new_feature
```
```
% git log --all --decorate --oneline
4a0ef84 (origin/new_feature) small change to readme
8b50a49 (origin/another_fix_branch) small change to readme
f243f91 (HEAD -> master, origin/master, origin/HEAD) Merge branch 'master' \ of /home/rick/work/math
... log lines omitted ...
＜和訳＞
4a0ef84 (origin/new_feature) readme の小さな変更
8b50a49 (origin/another_fix_branch) readme に小さな変更
f243f91 (HEAD -> master, origin/master, origin/HEAD) /home/rick/work/math のブランチ 'master' をマージしました。
... ログ行省略 ...
```

### 3. FETCH_HEAD は、現在のブランチのリモート追跡ブランチと一致するように変更されます。前回の練習から続けると、FETCH_HEAD の SHA1 ID が master と一致することがわかります。master は変更されていないので、マージはできません。

```
% git rev-parse FETCH_HEAD
f243f91c5eaed9f4ef076218a23768720ce484a0
```
```
% git rev-parse master
f243f91c5eaed9f4ef076218a23768720ce484a0
```
```
% git merge FETCH_HEAD
Already up to date.
＜和訳＞
すでに最新だ。
```

しかし、ブランチを変更しても、FETCH_HEADは変わらない！

```
% git checkout new_feature
Branch 'new_feature' set up to track remote branch 'new_feature' from 'origin'.
Switched to a new branch 'new_feature'
＜和訳＞
ブランチ 'new_feature' は、'origin' からのリモートブランチ 'new_feature' を追跡するように設定されました。
新しいブランチ 'new_feature' に切り替わった
```
```
% git rev-parse FETCH_HEAD
f243f91c5eaed9f4ef076218a23768720ce484a0
```

FETCH_HEADの変更を見るには、もう一度git fetchをする必要があります。詳しく見ていきましょう。

```
% cd ../math.bill
```
```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is up to date with 'origin/new_feature'.
＜和訳＞
ブランチ 'new_feature' に切り替えました。
ブランチが 'origin/new_feature' に更新されました。
```
```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme 2"
[new_feature 534de35] small change to readme 2
1 file changed, 1 insertion(+)
```
```
% git push
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 306 bytes | 306.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To /home/rick/work/math.git
4a0ef84..534de35 new_feature -> new_feature
```
```
% cd ../math.carol
```
```
% git fetch
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /home/rick/work/math
4a0ef84..534de35 new_feature -> origin/new_feature
```
```
% git rev-parse FETCH_HEAD
534de35ace4c11ae118eee807376e863d5662dcf
```
今回、git fetchによってFETCH_HEADが変更された。
```
% git checkout another_fix_branch
Branch 'another_fix_branch' set up to track remote branch 'another_fix_branch' \
from 'origin'.
Switched to a new branch 'another_fix_branch'
＜和訳＞
ブランチ 'another_fix_branch' が 'origin' からのリモートブランチ 'another_fix_branch' を追跡するように設定されました。
新しいブランチ 'another_fix_branch' に切り替わりました。
```
```
% git branch
* another_fix_branch
master
new_feature
```

math.billの変更をもう少しコミットして、git pushを実行しよう。

```
% cd ../math.bill
```
```
% git branch
another_fix_branch
master
* new_feature
```
```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme 3"
[new_feature 2fd390d] small change to readme 3
1 file changed, 1 insertion(+)
```
```
% git checkout another_fix_branch
Switched to branch 'another_fix_branch'
Your branch is up to date with 'origin/another_fix_branch'.
＜和訳＞
ブランチ 'another_fix_branch' に切り替えました。
ブランチが 'origin/another_fix_branch' に更新されました。
```
```
% echo "small change" >> readme.txt
```
```
% git commit -a -m "small change to readme 3"
[another_fix_branch b038c52] small change to readme 3
1 file changed, 1 insertion(+)
```
```
% git push --all
Counting objects: 37, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (33/33), done.
Writing objects: 100% (37/37), 3.15 KiB | 806.00 KiB/s, done.
Total 37 (delta 18), reused 0 (delta 0)
To /home/rick/work/math.git
534de35..2fd390d new_feature -> new_feature
8b50a49..b038c52 another_fix_branch -> another_fix_branch
! [rejected] master -> master (fetch first)
error: failed to push some refs to '/home/rick/work/math.git'
＜和訳＞
error: '/home/rick/work/math.git' への参照のプッシュに失敗しました。
```

前回と同様、masterに関する警告（「fetch first」）が表示されました。では、math.carolに戻ってgit fetchしてみましょう。

```
% git branch
* another_fix_branch
master
new_feature
```
```
% git fetch
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 3), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
From /home/rick/work/math
8b50a49..b038c52 another_fix_branch -> origin/another_fix_branch
534de35..2fd390d new_feature -> origin/new_feature
```
```
% git rev-parse FETCH_HEAD
b038c52d42a6e0b6f4512bba3e33ecec1cc52ed3
```

math.carol リポジトリで git fetch を実行すると、現在いるブランチ (another_fix_branch) の FETCH_HEAD が更新されます。変更を加えた別のブランチに切り替えるとどうなるでしょうか?

```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is behind 'origin/new_feature' by 2 commits, and can be fast-forwarded.
(use "git pull" to update your local branch)
＜和訳＞
ブランチ 'new_feature' に切り替えました。
あなたのブランチは 'origin/new_feature' から 2 コミット遅れています。またfast-forwardできます。
(ローカルブランチを更新するには "git pull" を使用します)。

```
```
% git rev-parse FETCH_HEAD
b038c52d42a6e0b6f4512bba3e33ecec1cc52ed3
FETCH_HEAD didn’t change! But we can update it by running git fetch one more time.
＜和訳＞
FETCH_HEADは変わっていない！しかし、もう一度 git fetch を実行すれば更新できます。
```
```
% git fetch
```
```
% git rev-parse FETCH_HEAD
2fd390deb908e19165a1d218775e7fafc1725778
```

これは興味深い行動であり、注意すべきことだと思う！

### 4. これは思考実験のようなものだが、これらのコマンドを実行することで、何が起きているのかがより明確になるだろう。math.carolリポジトリに入ったら、次のようにタイプする：

```
% git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
＜和訳＞
ブランチを 'master' に切り替えました。
ブランチが 'origin/master' に更新されました。
```
```
% git remote
origin
```
```
% git remote -v show
origin /home/rick/work/math.git (fetch)
origin /home/rick/work/math.git (push)
```
```
% git remote add bill ../math.bill
```
```
% git remote -v show
bill ../math.bill (fetch)
bill ../math.bill (push)
origin /home/rick/work/math.git (fetch)
origin /home/rick/work/math.git (push)
```
```
% git branch --set-upstream-to=bill/master
error: the requested upstream branch 'bill/master' does not exist
hint:
hint: If you are planning on basing your work on an upstream
hint: branch that already exists at the remote, you may need to 
hint: run "git fetch" to retrieve it.
hint:
hint: If you are planning to push out a new local branch that
hint: will track its remote counterpart, you may want to use
hint: "git push -u" to set the upstream config as you push.
＜和訳＞
error: 要求されたアップストリームブランチ 'bill/master' が存在しません。
hint:
ヒント: すでにリモートに存在するアップストリームブランチをベースに作業を進める場合は、"git fetch "を実行してブランチを取得する必要があるかもしれません。
hint:
ヒント: リモートブランチを追跡する新しいローカルブランチをプッシュする場合は、"git push -u" を使用してプッシュ時にアップストリームの設定を行うことができます。
```

この本の現行版では、-set-upstream-toスイッチを使う前に1つだけコマンドを忘れていました：git fetch billです。この git fetch を実行すると、bill という名前のリモートのトラッキングブランチが表示されます。git ブランチのエラーメッセージが、このことを教えてくれます。git fetch の前にブランチとリモートを調べるには、セッションを次のようにします：

```
% git branch --all
another_fix_branch
* master
new_feature
remotes/origin/HEAD -> origin/master
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature
```
```
% git branch -vv
another_fix_branch 10db974 [origin/another_fix_branch] Small change
* master 97d4104 [origin/master] Added a new line

% git remote -v show bill
* remote bill
Fetch URL: ../math.bill
Push URL: ../math.bill
HEAD branch: master
Remote branches:
another_fix_branch new (next fetch will store in remotes/bill)
master new (next fetch will store in remotes/bill)
new_feature new (next fetch will store in remotes/bill)
Local refs configured for 'git push':
another_fix_branch pushes to another_fix_branch (local out of date)
master pushes to master (local out of date)
new_feature pushes to new_feature (local out of date)
```
```
% git fetch bill
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
From ../math.bill
* [new branch] another_fix_branch -> bill/another_fix_branch
* [new branch] master -> bill/master
* [new branch] new_feature -> bill/new_feature
```
```
% git branch --all
another_fix_branch
* master
new_feature
remotes/bill/another_fix_branch
remotes/bill/master
remotes/bill/new_feature
remotes/origin/HEAD -> origin/master
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature
```
```
% git branch --set-upstream-to=bill/master
Branch 'master' set up to track remote branch 'master' from 'bill'.
＜和訳＞
ブランチ 'master' は 'bill' からのリモートブランチ 'master' を追跡するように設定されています。
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
* master f243f91 [bill/master: behind 2] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
＜和訳＞

```

math.carol が math.bill を指す独立したリモートを持つことの意味は、これら 2 つのリポジトリのユーザーが、一元化された math.git リポジトリを経由せずにお互いの作業を共有できるということです。第12.1.3章ではこの状況について説明しますが、git pushとgit pullについてわかったので、この状況を実際に調べてみましょう。実験が終わったら、アップストリームをリセットすることを忘れないでください。

```
% git branch --set-upstream-to=origin/master
Branch 'master' set up to track remote branch 'master' from 'origin'.
＜和訳＞
ブランチ 'master' は、'origin' からのリモートブランチ 'master' を追跡するように設定されています。
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
* master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
```

### 5. このマージの練習は、リモートトラッキングブランチを直接使えることを示しています。まずは new_feature ブランチからのマージを試してみましょう。

```
% git rev-parse new_feature
4a0ef848e2fd71d9579c7765fcb6224ba93b23d3
```
```
% git merge new_feature
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
＜和訳＞
コンフリクト（内容）： readme.txtのコンフリクトのマージ
コンフリクトを修正してから結果をコミットしてください。
```
```
% cat readme.txt
This is a README file. Enjoy.
<<<<<<< HEAD
A small update.
=======
small change
>>>>>>> new_feature
```
```
% git merge --abort
We now abandon this merge, and try a merge using the remote tracking branch origin/new_feature directly.
＜和訳＞
私たちはこのマージを断念し、リモート追跡ブランチ origin/new_feature を使って直接マージを試みます。
```
```
% git rev-parse origin/new_feature
2fd390deb908e19165a1d218775e7fafc1725778
```
```
% git merge origin/new_feature
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
＜和訳＞
コンフリクト（内容）： readme.txtのコンフリクトのマージ
コンフリクトを修正してから結果をコミットしてください。
```
```
% cat readme.txt
This is a README file. Enjoy.
<<<<<<< HEAD
A small update.
=======
small change
small change
small change
>>>>>>> origin/new_feature
```
```
% git merge --abort
```

ブランチ new_feature と origin/new_feature は異なるものです。なぜなら、new_feature ブランチでは git fetch で同期させてから git merge していないからです。このセクションでは、第 10.3.4 章で紹介した git merge --abort を使用しました。

origin/new_featureリモートブランチで新しいブランチを作成するには、次のようにする：

```
% git checkout -b new_new origin/new_feature
Branch 'new_new' set up to track remote branch 'new_feature' from 'origin'.
Switched to a new branch 'new_new'
＜和訳＞
ブランチ 'new_new' が 'origin' からのリモートブランチ 'new_feature' を追跡するように設定されました。
新しいブランチ 'new_new' に切り替わりました。
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
* new_new 2fd390d [origin/new_feature] small change to readme 3
```

これら2つのローカルブランチ（new_featureとnew_new）は現在、origin/new_featureという同じ上流を持ちます。これは混乱を招く可能性があります！より安全でわかりやすい方法は、git checkout で new_feature をチェックアウトし、そこから新しいブランチを作成することです。

```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is behind 'origin/new_feature' by 2 commits, and can be fast-forwarded.
(use "git pull" to update your local branch)
＜和訳＞
ブランチ 'new_feature' に切り替えました。
あなたのブランチは 'origin/new_feature' から 2 コミット遅れています。
(ローカルブランチを更新するには "git pull" を使用します)。
```
```
% git checkout -b new_new_new
Switched to a new branch 'new_new_new'
＜和訳＞
新しいブランチ 'new_new_new' に切り替えた。
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
new_new 2fd390d [origin/new_feature] small change to readme 3
* new_new_new 4a0ef84 small change to readme
＜和訳＞

```

この最新ブランチにはアップストリームがありませんが、new_new_newをオリジンにプッシュすると正しく設定されます。試してみてください！

### 6. FETCH_HEADの内容を見るには、'cat'を使う。セッションは次のようになる：

```
% cd $HOME/math.carol
```
``` 
% cat .git/FETCH_HEAD
f243f91c5eaed9f4ef076218a23768720ce484a0 branch 'master' of
/home/rick/work/math
b038c52d42a6e0b6f4512bba3e33ecec1cc52ed3 not-for-merge branch
'another_fix_branch' of /home/rick/work/math
2fd390deb908e19165a1d218775e7fafc1725778 not-for-merge branch
'new_feature' of /home/rick/work/math
```

FETCH_HEADの中身についてのコメントはありません。Gitが内部のデータを使って私たちに選択肢を提示しているのは明らかですが、Gitのソースコードを読まない限り、その詳細は隠されています。
