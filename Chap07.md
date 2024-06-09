Chapter 7
Ch 7.5.1
The instructions for this lab got truncated for some reason! This exercise was to make a Git repository
containing the file lorem-ipsum.txt, and then to introduce some changes to that file (copying the contents of
loreum-ipsum-change.txt on top of lorem-ipsum.txt).
If you decipher the instructions, the output from git diff will look like the following:
diff --git a/lorem-ipsum.txt b/lorem-ipsum.txt
index dd194e9..b3ee228 100644
--- a/lorem-ipsum.txt
+++ b/lorem-ipsum.txt
@@ -1,7 +1,7 @@
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc facilisis
venenatis felis, scelerisque consectetur lacus viverra in. Phasellus
scelerisque pretium nibh, ac porttitor massa vestibulum non. Aenean ornare
-convallis enim, et semper neque sagittis ut. Proin blandit dui vitae quam
+convallis enim, et change here. Proin blandit dui vitae quam
fermentum, non lacinia lectus molestie. Vivamus tempus viverra dui. Cras in
luctus felis. Pellentesque dictum tortor ipsum, id iaculis felis ultrices ut.
Nunc id velit dignissim, suscipit est ac, euismod libero. Phasellus fringilla,
@@ -10,8 +10,7 @@ mi sagittis odio. Donec fermentum sem vel neque pulvinar eleifend.
Nullam posuere condimentum rutrum. Vivamus vitae elit a tellus interdum
accumsan. Duis pretium viverra condimentum. Curabitur dapibus aliquet semper.
-Aliquam pretium nunc aliquam pellentesque molestie. Suspendisse potenti. In a
-erat sed ante rutrum commodo. Fusce congue nunc aliquet tellus dignissim
+Aliquam pretium nunc deleted below molestie. Suspendisse potenti. In a
fringilla.
4
Cras non odio gravida, blandit risus sed, tempor lectus. Donec vel congue
@@ -28,6 +27,7 @@ sem convallis, nec vulputate ante ullamcorper. Nam laoreet, sapien in fermentum
elementum, ante massa vehicula eros, non luctus ipsum leo a metus.
Nunc velit lacus, pellentesque sit amet libero ac, rutrum facilisis est. Ut
+added a new line here do not be afraid added new line here do not be afraid
aliquam justo sit amet ornare condimentum. Curabitur nibh est, vehicula at
sodales non, posuere in quam. Quisque ornare rutrum arcu ut porttitor. Nunc
blandit justo eget nisl accumsan, eu auctor urna pretium. Proin non condimentum
If your repository can produce the above git diff output, you’re ready to tackle the questions in the
exercise.
If you’re stuck, the answers are below. See if you can work backwards from this!
1. There are three hunks (they are delimited by @@ at the start of the line).
2. The second hunk’s @@ string should be “-10,8 +10,7”. If you stare at the difference, you’ll see that a
line was deleted. To stage and commit this change, use:
git add -p
At this point, you’ll be prompted with “Stage this hunk [y,n,q,a,d,/,K,j,J,g,e,?]?” for each hunk. For
the first hunk, reply with ‘n’. For the second hunk, reply with ‘y’. For the third hunk, reply with ‘n’.
At this point, git status will show that the file lorem-ipsum.txt has staged and unstaged changes.
Commit the staged changes by typing git commit -m "2nd hunk".
3. To check out the latest code, git status will suggest git checkout -- <file>... to discard changes
in the working directory. Translate this to git checkout -- lorem-ipsum.txt.
After typing this command, git status should report that the working directory is clean, with nothing
to commit.
4. This step is a repeat of the setup instructions, but this time the file in our repository is different.
5. There are two hunks.
6. To commit the entire file, there’s no need to select each hunk one by one. Just type git add
lorem-ipsum.txt, then git commit -m "All hunks".
Ch 7.5.2
Deleting files in Git require git rm. In this exercise, I have you perform a git rm, but then I encourage
you to revert the changes, following the git status commands. However, as I write this, reverting the file
requires two commands: git reset and git checkout. Each time git status shows us the syntax.
My command line session for doing this follows:
% ls
lorem-ipsum.txt
% git rm lorem-ipsum.txt
rm 'lorem-ipsum.txt'
% ls
% git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
5
deleted: lorem-ipsum.txt
% git reset HEAD lorem-ipsum.txt
Unstaged changes after reset:
D lorem-ipsum.txt
% ls
% git status
On branch master
Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
deleted: lorem-ipsum.txt
no changes added to commit (use "git add" and/or "git commit -a")
% git checkout -- lorem-ipsum.txt
% ls
lorem-ipsum.txt
% git status
On branch master
nothing to commit, working directory clean
Ch 7.5.3
The word ‘form’ is used in the Git documentation to indicate the pattern of the arguments.
The git checkout -- math.sh form is formally described in the git checkout as git checkout
[-p|--patch] [<tree-ish>] [--] <pathspec>... Notice that the brackets indicate optional arguments.
In our command, we omit the --patch and [tree-ish]. Knowing this form leads you to the right section of
the manual to read.
The git reset math.sh form is described in git reset as git reset [-q] [<tree-ish>] [--]
<paths>.... In the command we give, we omit the -q and the [tree-ish] arguments.
Both commands specify -- as an optional argument. For both our commands, we omit --. These doubledashes
are used to separate the tree-ish from the paths. The book doesn’t discuss tree-ish, or a use-case
involving --, but you can visit this Stack Overflow URL for more.
http://stackoverflow.com/q/13321458/10030
