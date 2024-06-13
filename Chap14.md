## Chapter 14

### 1. この練習の意図は、14.2.2項のようにgit mergeを使うことです。

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

### 3. FETCH_HEAD changes to match the current branch’s remote tracking branch. If you continue from the last exercise, you’ll see that FETCH_HEAD’s SHA1 ID matches master. There’s no merge possible, since master didn’t change.

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
```

However, if we change our branch, the FETCH_HEAD stays the same!

```
% git checkout new_feature
Branch 'new_feature' set up to track remote branch 'new_feature' from 'origin'.
Switched to a new branch 'new_feature'
```
```
% git rev-parse FETCH_HEAD
f243f91c5eaed9f4ef076218a23768720ce484a0
```

To see FETCH_HEAD change, we have to do a git fetch again. Let’s go over it in detail.

```
% cd ../math.bill
```
```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is up to date with 'origin/new_feature'.
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
% cd ../math.carol/
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
This time, git fetch caused FETCH_HEAD to change.
```
```
% git checkout another_fix_branch
Branch 'another_fix_branch' set up to track remote branch 'another_fix_branch' \
from 'origin'.
Switched to a new branch 'another_fix_branch'
```
```
% git branch
* another_fix_branch
master
new_feature
```

Let’s commit a few more changes on math.bill, and perform a git push.

```
% cd ../math.bill/
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
```

As before, we saw the warning about master (“fetch first”). Now let’s go back to math.carol, and do
git fetch.

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

In the math.carol repository, doing a git fetch will update the FETCH_HEAD of the current branch that we’re on (another_fix_branch). What happens when we switch to the other branch that has changes?

```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is behind 'origin/new_feature' by 2 commits, and can be fast-forwarded.
(use "git pull" to update your local branch)
```
```
% git rev-parse FETCH_HEAD
b038c52d42a6e0b6f4512bba3e33ecec1cc52ed3
FETCH_HEAD didn’t change! But we can update it by running git fetch one more time.
```
```
% git fetch
```
```
% git rev-parse FETCH_HEAD
2fd390deb908e19165a1d218775e7fafc1725778
```

This strikes me as an interesting behavior, and something to watch out for!

### 4. This is more of a thought experiment, but performing these commands will make it more clear what is happening. Once you get into the math.carol repository, type the following:

```
% git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
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
```

In the current edition of the book, I forgot one command before using the --set-upstream-to switch: git fetch bill. Running this git fetch will bring in the remote tracking branches for the remote named bill. The error message from the git branch helpfully suggests this. To study the branches and remotes before git fetch, your session might look like this:

```
% git branch --all
another_fix_branch
* master
new_feature
remotes/origin/HEAD -> origin/master
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature

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
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
* master f243f91 [bill/master: behind 2] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
```

The implication of math.carol having a separate remote pointing to math.bill is that the users of these two repositories can share work between each other without going through the centralized math.git repository. Chapter 12.1.3 discusses this situation, but now that we know about git push and git pull, you can really explore this situation. Don’t forget to reset the upstream after experimenting.

```
% git branch --set-upstream-to=origin/master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
* master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
```

### 5. This merge exercise shows that you can use the remote tracking branches directly. First, let’s attempt a merge from the new_feature branch.

```
% git rev-parse new_feature
4a0ef848e2fd71d9579c7765fcb6224ba93b23d3
```
```
% git merge new_feature
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
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

The branches new_feature and origin/new_feature are different, because we haven’t sync’d them with a git fetch then git merge in the new_feature branch. This section had us using the git merge --abort, which was introduced in Chapter 10.3.4.

To create a new branch with the origin/new_feature remote branch, you could do:

```
% git checkout -b new_new origin/new_feature
Branch 'new_new' set up to track remote branch 'new_feature' from 'origin'.
Switched to a new branch 'new_new'
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
* new_new 2fd390d [origin/new_feature] small change to readme 3
```

These two local branches (new_feature and new_new) now have the same upstream of origin/new_feature. This can be confusing! The safer, more clearer way is to git checkout
new_feature, and make a new branch from that using the technique from Chapter 9.3.1.

```
% git checkout new_feature
Switched to branch 'new_feature'
Your branch is behind 'origin/new_feature' by 2 commits, and can be fast-forwarded.
(use "git pull" to update your local branch)
```
```
% git checkout -b new_new_new
Switched to a new branch 'new_new_new'
```
```
% git branch -vv
another_fix_branch 8b50a49 [origin/another_fix_branch: behind 1] small change to readme
master f243f91 [origin/master] Merge branch 'master' of /home/rick/work/math
new_feature 4a0ef84 [origin/new_feature: behind 2] small change to readme
new_new 2fd390d [origin/new_feature] small change to readme 3
* new_new_new 4a0ef84 small change to readme
```

This newest branch doesn’t have an upstream, but when you push new_new_new to origin, it will be set properly. Give that a try!

### 6. To look at the contents of FETCH_HEAD, use ‘cat’. Your session would look like this:

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

I don’t have any commentary on FETCH_HEAD’s contents. Clearly Git is using the data inside to present choices to us, but those details are hidden, unless you read Git’s source code.
