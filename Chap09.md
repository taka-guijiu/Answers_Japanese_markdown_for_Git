Chapter 9
Ch 9.5.1
1. Git GUI has a decent mechanism to check out branches.
First, you select the Branch menu item, as in the following figure.
10
Figure 2: gitk and the lots_of_commits repository
11
Then, on the resulting branch selector dialog window, the next figure, select “new_feature_branch”,
and click the Checkout button.
After doing the above steps, your repository will be in the new_feature_branch branch.
2. gitk has a mechanism for making branches off of an existing branch.
12
First, use the right-mouse button click (context menu) on the commit text of the branch that you
want to use (in this exercise, it was ‘another_fix_branch’), as shown in the next figure. (Selecting the
context menu by clicking on the branch name presents a different menu.)
From the context menu, select “Create a new branch”. A smaller window appears, allowing you to enter
the name of a branch (see the following picture). I’ve entered “new_one.”
13
After you press “Create”, the new branch will appear in gitk, alongside another_fix_branch (as shown
in the next figure).
3. This exercise is a repeat of the previous two exercises. gitk is the easier of the two tools to create a
branch off a tag. In Git GUI, in the “Create Branch” window, you must select the “Tag” button to
present the list of available tags (next figure).
4. This exercise asks you to delete these branches using the two GUIs. Both are relatively straightforward.
For Git GUI, you select Delete from the Branch menu (see the first figure of this section for the branch
menu items). Then, in the Delete window (in the following picture), pick the branch to delete, then
press “Delete.”
14
For gitk, this time right-mouse button click on the branch name. You’ll see a menu item to remove
the branch (as shown in the next figure).
Ch 9.5.2
1. You can rename a branch, via the git branch -m old_name new_name command.
15
2. We saw this technique earlier (in 8.5.3’s second exercise). Use:
git rev-parse :/"Renaming c and d"
3. As a reminder, the lengthy git log was:
git log --graph --decorate --pretty=oneline --all --abbrev-commit
Looking up each switch involves using the git log --help command. The point of this exercise is to
have you read the git log documentation, specifically looking up particular switches.
For the case of the --decorate switch, you’ll see that it prints the ref names of any commit, but there
are switches to decorate. For example, try --decorate=full.
4. To give this a proper try, go to the math directory, and try deleting the new_feature branch. (You may
find it easier to regenerate the math directory using the make_math_repo.sh script.)
The new_feature branch has two commits in it: “Starting a second new file” and “Adding a new file to
a new branch”.
Once you type git branch -d new_feature, you will be asked whether you really want to delete this
branch, as there are commits that haven’t been merged to any other branch. To really delete the branch,
type git branch -D new_feature. (Notice that the second command uses the capital D, instead of a
lowercase d.)
Now that the repository no longer has a new_feature branch, you might think that your commits for
this branch are gone. However, they are still available, and the best way to see this is via git reflog.
Type git reflog, and you’ll see a listing that looks like this:
3522453 HEAD@{0}: checkout: moving from fixing_readme to another_fix_branch
3522453 HEAD@{1}: checkout: moving from master to fixing_readme
7fb5600 HEAD@{2}: commit: A small update to readme.
b06aec0 HEAD@{3}: checkout: moving from new_feature to master
c5aab93 HEAD@{4}: commit: Starting a second new file
6851702 HEAD@{5}: commit: Adding a new file to a new branch
b06aec0 HEAD@{6}: checkout: moving from master to new_feature
b06aec0 HEAD@{7}: commit: Adding printf.
In the listing above, you can see our two commits at the label HEAD@{4} and HEAD@{5}. You can
recreate a branch specifying this label, effectively recovering the two commits from the deleted branch!
Type:
git checkout -b recovered_branch HEAD@{4}
git reflog will be discussed some more in Chapter 16.
Ch 9.5.3
1. This question was originally asked as part of 7.5.3!
However, it is worth emphasizing the notion of ‘form’ again. For every Git command, the documentation
lists a synopsis of ways to call that particular Git command. For the git checkout command, this is
the synopsis.
git checkout [-q] [-f] [-m] [<branch>]
git checkout [-q] [-f] [-m] --detach [<branch>]
git checkout [-q] [-f] [-m] [--detach] <commit>
git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new_branch>] [<start_point>]
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <paths>...
git checkout [-p|--patch] [<tree-ish>] [--] [<paths>...]
16
Each line in the synopsis is a particular form of the git checkout command. As with 7.5.2, the answer
is git checkout [-p|--patch] [<tree-ish>] [--] [<pathspec>...]. In the current question, we
use the -- argument, but as we saw in 7.5.2, this is optional.
2. One way to add the new line and commit the change is as follows:
% echo "c=1" >> math.sh
% git add math.sh
% git commit -m "Adding a third variable."
After the above steps, examine the repository via gitk (and the “All (local) branches” view configured).
It should look like 9.23 in the book!
Ch 9.5.4
1. All the branches originate from the commit with the message “Adding four empty files.” This commit
is tagged with “four_files_galore”. To get this answer, you can use gitk, or use the git lol command
and scroll all the way to the bottom.
2. The answer is random, on every run of the make_lots_of_branches.sh script. To get the right answer,
type git checkout branch_30, then examine the output of git log --oneline --decorate. It
should look as follows.
% git log --oneline --decorate --graph
* 7d3202f (HEAD -> branch_30) Commit for file_26611
* 5fe1ae0 Commit for file_20400
* 4c4316b Commit for file_25981
* 19d8bb1 Commit for file_16225
* 8bb7993 Commit for file_735
* 19a25f2 (tag: four_files_galore, master) Adding four empty files.
* 3cb4cd5 Adding b variable.
* 0967ff6 This is the second commit.
* 446143d This is the first commit.
In the above examine, it shows five commits. You can also see this via gitk (again with the “All (local)
branches” configuration.
3. There are lots of ways to approach this.
The brute force way is to use gitk, enable “All (local) branches”, and then scroll through the entire list
of commits.
Another way to do this is to exploit the command line. You can use:
% git log --decorate --all |grep random_prize
commit 72dee077a500c7f7a678a24703cff04ef0ac3e13 (tag: random_prize_1, branch_22)
commit f07b28eda8c17868789b817371541bba8c18cc9f (tag: random_prize_2, branch_26)
commit 774a8cd97afe3e54427817bd25df1115cc2209b5 (tag: random_prize_3, branch_05)
The above command line takes the output of git log, and searches it via the grep command. (This is
similar to the technique used in question 3 of 8.5.6.)
One more way to get the random branches is to use git rev-parse. Your session would look something
like this:
% git rev-parse --tags=random_prize_*
d8d1ecc4b7e988b0daf176a3615665ef5101bc1b
aed1e7e201189636c84708f2b1420e07f93905e5
81527e46382b91f07b48f2c2871e0f7ab84aa5de
Then for every SHA1 ID, type git log, as follows:
17
% git log --decorate -1 d8d1ecc
commit 72dee077a500c7f7a678a24703cff04ef0ac3e13 (tag: random_prize_1, branch_22)
Author: Rick Umali <rickumali@gmail.com>
Date: Tue Dec 1 21:35:44 2015 -0500
Commit for file_6774
To check your answers, type git checkout branch_40, then examine the answers.txt file in that
branch. (Notice that this file is only in this branch!)
4. The hint was the word ‘contains’. git branch has a --contains switch that you can use to find any
branch that contains a particular pattern (in this case, our tag name). The command line would be:
% git branch --contains random_tag_on_file
branch_21
For the above, the random_tag_on_file is in the branch_21. (Yours will be different, potentially!)
5. Typing the command in this exercise will produce a lengthy listing of all the branches and tags only.
% git log --oneline --decorate --simplify-by-decoration --all
62240e1 (branch_32) Commit for file_3625
...
06c2269 (branch_39) Commit for file_28520
da81290 (HEAD -> branch_40) The answers are in this commit.
72dee07 (tag: random_prize_1, branch_22) Commit for file_6774
58c4052 (branch_23) Commit for file_341
dc71c38 (branch_24) Commit for file_6528
22f2527 (branch_25) Commit for file_7970
f07b28e (tag: random_prize_2, branch_26) Commit for file_8285
002b174 (branch_27) Commit for file_12577
a5c4356 (branch_28) Commit for file_20648
...
8c48a4c (branch_21) Commit for file_22333
195b7d0 (tag: random_tag_on_file) Randomly tagged file on Branch 21.
8fa867f (branch_06) Commit for file_23011
...
2da5dab (branch_04) Commit for file_22551
774a8cd (tag: random_prize_3, branch_05) Commit for file_21948
19a25f2 (tag: four_files_galore, master) Adding four empty files.
446143d This is the first commit.
In the output above, I used ‘. . . ’ to indicate rows that I left out for space. The key is that the output
only contains commits that are either a branch or a tag (or both). This kind of history listing is known
as a ‘simple history’.
When you add the --graph switch, you’ll see the same listing, with some ASCII branches indicating
that all branches stem from the commit tagged ‘four_files_galore’. (See and compare with the first
question of this section.)
