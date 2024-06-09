Chapter 10
1. To access the git merge command’s help file, type git mege --help.
2. It might be helpful, if your mergesample directory is a little disjointed, to recreate it using the
make_merge_repos.sh script.
18
This exercise asks you to merge ‘master’ into your branch. An example of this from scratch would look
like this:
% git branch
* bugfix
master
% git checkout master
Switched to branch 'master'
% git checkout -b newbranch
Switched to a new branch 'newbranch'
% git merge master
Already up-to-date.
The above session is the simplest situation. A newly created branch that merges in an unchanged
master. Since nothing is changed in either branch, master is effectively merged with newbranch. To
make things more realistic, add a file to newbranch:
% touch innb
% git add innb
% git commit -m "Adding innb file"
[newbranch fb0c9da] Adding innb file
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 innb
% git merge master
Already up-to-date.
Does this response from git merge master make sense? Since master does not have any changes, it’s
already ‘merged’ with newbranch. newbranch is a direct-descendant of master. Review the diagrams in
section 10.4.1 to refresh your memory of this concept.
Now, to really test your understanding, try to make a change to master.
% git checkout master
Switched to branch 'master'
% touch nil
% git add nil
% git commit -m "Adding nil file"
[master 4cb6caa] Adding nil file
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 nil
% git checkout newbranch
Switched to branch 'newbranch'
% git merge master
Merge made by the 'recursive' strategy.
nil | 0
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 nil
Since master has a change, Git has to merge it into newbranch. You may find yourself in this situation
when you have a branch off of master, but then the master branch has a change. In order to bring in
the changes from master, you could do this kind of merge, although we’ll see other techniques to keep
19
track of master in the chapter on git rebase.
3. “What happens if you type it now?” This isn’t a great exercise question because I don’t say what
step to start from. Recreating the mergesample directory using the make_merge_repos.sh script will
produce the desired state for exploring the third question: “What happens to the diff if you try a
different order?”
% git diff master...bugfix
diff --git a/baz b/baz
index 56d6546..1c52108 100644
--- a/baz
+++ b/baz
@@ -1,3 +1,4 @@
a=1
-b=0
+b=1
let c=$a/$b
+echo $c
% git diff bugfix...master
diff --git a/bar b/bar
new file mode 100644
index 0000000..e69de29
diff --git a/foo b/foo
new file mode 100644
index 0000000..e69de29
The “. . . ” says what is the difference between master and branch relative to when they first became
different. The output shows how to make master branch look like the bugfix branch. This is why the
diff output looks so different when you reverse the two branches. To make bugfix look like master, you
only have to add two new files: bar and foo.
The question “Is there another word you can substitute for either master or bugfix?” points to the fact
that any gitrevisions substitute for master or bugfix is allowed. The easiest example is HEAD can
be substituted for either master or bugfix. From the git diff help page: “For a more complete list of
ways to spell commit, see SPECIFYING REVISIONS section in gitrevisions.”
4. After you add (and commit) a new file to the bugfix branch, doing git diff master...bugfix will
show the output from listing 10.2, with the following entry specifying that a new file is also needed (in
our example, the innb file):
diff --git a/innb b/innb
new file mode 100644
index 0000000..e69de29
The question asks about the --name-status output, and it would look like this:
% git diff --name-status master...bugfix
M baz
A innb
Here, it’s apparent that innb is an addition to the branch.
5. To add a commit even though you’re performing fast-forward merge, use the --no-ff switch. This is
switch we’ll explore further in Chapter 17.
20
