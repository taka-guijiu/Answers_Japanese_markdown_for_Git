Chapter 8
git log is a command that people should feel completely comfortable experimenting with. It’s read-only,
and it doesn’t do any writing to the repository!
Ch 8.5.1
1. git log --reverse
6
In this chapter, we learned that each commit points back to its parent. git log has a --parents
switch that helps facilitate viewing the parents. To me, this suggests that it is much easier for Git
to generate the default git log listing, than it is to generate the listing required by the --reverse
switch. This is more apparent on a very large repository. For the Git source code, git log --reverse
pauses for a few seconds before displaying the first commit.
2. git log -N (where N is any number)
This limits the git log output to N commits. -N is the shortest form of this command, which can also
be expressed as -n N or --max-count=N.
3. git log --relative-date
4. git log --pretty=oneline --abbrev-commit
Ch 8.5.2
Below is a session that follows the instructions in this exercise.
% echo "new line" >> readme.txt
% git add readme.txt
% git commit -m "One new line"
[master 0ce6b5a] One new line
1 file changed, 2 insertions(+)
% git log -1
commit 0ce6b5aa98e69fc5f34122ff479fa68842a52987
Author: Rick Umali <rickumali@gmail.com>
Date: Thu Nov 5 20:10:45 2015 -0500
One new line
% git commit --amend -m "Fixed commit" -m "Second paragraph" -m "Wall of text"
[master bf176fd] Fixed commit
Date: Thu Nov 5 20:10:45 2015 -0500
1 file changed, 2 insertions(+)
% git log -1
commit bf176fd3acab541a84f1f6a35137b9ae61d789a2
Author: Rick Umali <rickumali@gmail.com>
Date: Thu Nov 5 20:10:45 2015 -0500
Fixed commit
Second paragraph
Wall of text
Given the output above, you can see that SHA1 ID is different from the first time you performed the commit.
git commit --amend causes the SHA1 IDs to change!
Ch 8.5.3
1. The first three commands return the third ancestor from the most recent commit on master.
If your log looks like this (on the master branch):
7
% git log --oneline -4
c0fd19e A small update to readme.
a853319 Adding printf.
f646129 Adding two numbers.
6a476a7 Renaming c and d.
The command git rev-parse master~3 returns:
6a476a73bbb608c4efa508b7a13509b38f8c93f7
Note that the rev-parse returns the full SHA1 ID. To mimic the display of --oneline, pass --short to
the rev-parse command.
Both git show commands will show the same output, because master@{3} and masterˆˆˆ are synonyms.
2. The git rev-parse :/"Removed a and b." will search the commit logs, and return the SHA1 ID of
the commit that contains the string “Removed a and b.” This is a nifty trick, and we’ll see a variant of
this in Chapter 15.
3. Yes! Type git rev-parse four_files_galore, and you will see the SHA1 ID that this tag points to.
4. Yes, all the funky syntax of git rev-parse work on tag names as well. Type git rev-parse
four_files_galoreˆˆ, and it will return the SHA1 ID that is commits before four_files_galore.
Ch 8.5.4
Below is a session that follows the instructions in this exercise.
% git checkout four_files_galore
Note: checking out 'four_files_galore'.
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:
git checkout -b <new-branch-name>
HEAD is now at 84965d0... Adding four empty files.
% ls
a b c d math.sh
% echo -n "commit" >> d
% git add d
% git commit -m "One commit to d"
[detached HEAD 399cd3f] One commit to d
1 file changed, 1 insertion(+)
% git checkout master
Warning: you are leaving 1 commit behind, not connected to
any of your branches:
8
399cd3f One commit to d
If you want to keep it by creating a new branch, this may be a good time
to do so with:
git branch <new-branch-name> 399cd3f
Switched to branch 'master'
From that last git checkout command, Git noticed that you were leaving a stray commit behind. In the
next chapter, we’ll show a better way for creating commits so that they’re not strays!
Ch 8.5.5
To delete a tag, use the -d switch to git tag. A session showing off this would look like the following.
% git branch
another_fix_branch
* master
new_feature
% git log --oneline -3
c0fd19e A small update to readme.
a853319 Adding printf.
f646129 Adding two numbers.
% git tag -m "Next to last" penultimate master^^
% git rev-parse penultimate
5edd4a3238c288c329126f0144a28df238ef275c
% git show penultimate
tag penultimate
Tagger: Rick Umali <rickumali@gmail.com>
Date: Thu Nov 5 20:54:00 2015 -0500
Next to last
commit f6461290964d974e807d0fd3dbf1c041270dc3a4
Author: Rick Umali <rickumali@gmail.com>
Date: Wed Nov 4 20:17:25 2015 -0500
Adding two numbers.
diff --git a/math.sh b/math.sh
index 5bb7f63..dab42fb 100644
--- a/math.sh
+++ b/math.sh
@@ -1,3 +1,5 @@
-# Comment
+# Add a and b
a=1
b=1
% git tag
9
four_files_galore
penultimate
% git tag -d penultimate
Deleted tag 'penultimate' (was 5edd4a3)
% git tag
four_files_galore
We incorporated the funky syntax to pick a commit that is two commits behind master. If it wasn’t clear
earlier, notice that the git rev-parse penultimate command returns the SHA1 ID of the tag, but git
show shows only the SHA1 ID of the commit that the tag points to.
Ch 8.5.6
This exercise has you running a script that creates a repository that contains many commits. This repository
provides another test area to try out the techniques from this chapter.
1. The very first commit says “The very first commit. Hi!” It should be five days old, meaning if you ran
the script on October 8, the date of this first commit would be October 3.
2. This question required you to use git rev-parse :/"ubiquitous". See 8.5.3 of this PDF to refresh
your memory!
3. git log --author=rgu
The translator for the Japanese version of the book offered the above concise solution.
When I originally wrote this answer, I found the answer by using git log, and within the pager
command, searching for the e-mail address, using “/”. Remember that Git displays most of its output
inside of a pager if it detects that the output will be more than a screen-ful of lines. You can search
this output using the pager.
You could also combine git log and the UNIX grep command to search for the answer! Try: git log
| grep -B 1 rgu@freeshell.
4. git log --since=yesterday
5. Your gitk output should look something like figure 2, examining the most recent commit. Clicking on
any file should show its contents in the various panes.
