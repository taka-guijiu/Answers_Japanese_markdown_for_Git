Chapter 4
1. You should see fatal: bad default revision 'HEAD'. I have seen reports of the error fatal: your
current branch 'master' does not have any commits yet.
1
2. Following the steps in this exercise, git status should give:
On branch master
Initial commit
Changes to be committed:
(use "git rm --cached <file>..." to unstage)
new file: file.txt
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
modified: file.txt
You should see file.txt listed twice.
3. The End of Line (EOL, aks newline) is the bane of computing. If you’re on a Windows, most files end
lines with CR+LF. If you’re on a Unix/Linux machine (including Mac), most files end with LF. The
Wikipedia page for Newline describes other variants.
If you spend all your time on one platform, you will not encounter this issue. However, with Git, you
may find yourself collaborating with platforms that are different from yours. Git attempts to be smart
about this, with the core.autocrlf and core.safecrlf configuration settings.
Read this post:
http://stackoverflow.com/q/170961/10030
on Stack Overflow for some detailed posts for how to handle EOL. This post contains recommendations
for managing EOL settings, but if you’re a beginner, I recommend skimming this material for now. Out
of the box, Git will try its best to manage EOL properly.
