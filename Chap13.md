Chapter 13
1. In the git push manual page, the section “Note about fast-forwards” is a concise summary of 13.2. If
you think hard about the first diagram:
B
/
---X---A
you should see that it’s the succinct version of Figure 13.6. The rest of the section proposes two ways
around this: git pull and git rebase.
This section proposes the use of git push --force when you’re absolutely sure no one else has modified
the remote since the last git push. It suggests one situation when this could occur: using git commit
25
--amend after doing a git push. We covered git commit --amend as a lab exercise in Chapter 8.5.2
(covered in this answer guide).
The key sentence in this whole section is “In other words, ‘git push –force’ is a method reserved for a
case where you do mean to lose history.” Keep this in mind!
2. The goal of this exercise is to review the ideas from 13.1.13 with a new clone. To create a new clone,
follow these steps in the same directory that contains the math.git directory (review Chapter 11 for
details on cloning).
% git clone math.git math.another
Cloning into 'math.another'...
done.
The above steps creates another clone which we’ve named math.another. Go into this directory, and
make a few commits.
% cd math.another/
% ls
another_rename math.sh readme.txt renamed_file
% echo "Adding one line" >> readme.txt
% git add readme.txt
% git commit -m "Added one line"
[master bd1a09d] Added one line
1 file changed, 1 insertion(+)
% echo "Adding another line" >> readme.txt
% git add readme.txt
% git commit -m "Added another line"
[master fb55793] Added another line
1 file changed, 1 insertion(+)
Now push up these changes to math.git, our origin.
% git push
Counting objects: 6, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 560 bytes | 0 bytes/s, done.
Total 6 (delta 3), reused 0 (delta 0)
To /home/rick/gitbook/math.git
352b88b..fb55793 master -> master
This changes math.git. math.git is in sync with math.another, but now the other repositories are out of
sync with math.git. To confirm this, go into another repository, and type git remote -v show. In
this sample session, we go into the math.carol repository.
% cd ../math.carol/
% git remote -v show origin
* remote origin
Fetch URL: /home/rick/gitbook/math.git
Push URL: /home/rick/gitbook/math.git
HEAD branch: master
Remote branches:
another_fix_branch tracked
master tracked
new_feature tracked
Local branches configured for 'git pull':
another_fix_branch merges with remote another_fix_branch
26
master merges with remote master
Local refs configured for 'git push':
another_fix_branch pushes to another_fix_branch (up to date)
master pushes to master (local out of date)
Notice the ‘local out of date’, which means the local repo (the one that you’re in) is out of sync with
the master on the origin, math.git.
3. The section of the git push help to read is OPTIONS. In this section, look for the paragraphs describing
<refspec>. It’s short description: “Specify what destination ref to update with what source object.”
This description also describes the “+”, which is similar to the --force option, which we experiment
with in the next question.
4. Continuing from the above session, let’s make some changes in the math.carol repository, and attempt
to push them up to the origin. In math.carol, we’re of out sync with the origin.
% git remote -v show origin
* remote origin
Fetch URL: /home/rick/gitbook/math.git
Push URL: /home/rick/gitbook/math.git
HEAD branch: master
Remote branches:
another_fix_branch tracked
master tracked
new_feature tracked
Local branches configured for 'git pull':
another_fix_branch merges with remote another_fix_branch
master merges with remote master
Local refs configured for 'git push':
another_fix_branch pushes to another_fix_branch (up to date)
master pushes to master (local out of date)
Let’s make a small change to readme.txt, and try to push it to the math.git (origin).
% echo "New line" >> readme.txt
% git add readme.txt
% git commit -m "Added a new line"
[master 97d4104] Added a new line
1 file changed, 1 insertion(+)
% git push
To /home/rick/gitbook/math.git
! [rejected] master -> master (fetch first)
error: failed to push some refs to '/home/rick/gitbook/math.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
Here, we encounter the situation described in 13.2, listing 13.8. To get around it forcibly, we now use
--force.
% git push --force
Counting objects: 9, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 892 bytes | 0 bytes/s, done.
Total 9 (delta 2), reused 0 (delta 0)
27
To /home/rick/gitbook/math.git
+ fb55793...97d4104 master -> master (forced update)
Notice the plus + sign in the last line (which says ‘forced update’).
The git push documentation describes the use of a plus + sign in the refspec, which seems to be the
equivalent of the --force switch, but there’s a subtle difference. Thankfully, this difference is spelled
out clearly in this post:
https://stackoverflow.com/questions/25937730/git-force-push-syntax-f-versus-branch
In practice, using --force is dangerous, as it deletes history! Coordinate with your team first if you
think you need --force.
5. The git push --tags syntax was introduced in 13.5. As with all command line options with Git, it
helps to read the official documentation. The --tags option will push “All refs under refs/tags are
pushed, in addition to refspecs explicitly listed on the command line.” The refs/tags directory is part of
the .git directory (see the Above and Beyond section in 4.2 and git help gitrepository-layout).
Any tag you create with git tag will have a corresponding file in this directory.
To see this for ourselves, let’s go to any repository (for this example, I’ll use the math.carol repo again).
% git tag -a tagone -m "tag one"
% git tag -a tagtwo -m "tag two"
% ls .git/refs/tags
tagone tagtwo
To push all these tags up to the origin, type:
% git push --tags
Counting objects: 2, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 203 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To /home/rick/gitbook/math.git
* [new tag] tagone -> tagone
* [new tag] tagtwo -> tagtwo
Notice that you didn’t have spell out each tag one by one! However, it’s a good practice to push tags
up individually, so that you avoid pushing tags that you only mean to use locally.
6. Remember that we created a remote to the math.bob repository in 12.1.3 in the math.carol repository.
When I type git push bob from the math.carol repository, I see the following:
% git push bob
To ../math.bob
! [rejected] master -> master (fetch first)
error: failed to push some refs to '../math.bob'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
This error is the same as question 4: math.bob and math.carol are not in sync. You may see other
errors, and if you do, please let me know.
7. The git config help page has a section called FILES. It’s a very helpful section, but it probably makes
sense to bookmark this until Chapter 20.1. The configuration push.default is in the long Variables
28
section of the git config help page, and it is worth reading to get the final word on what was convered
in 13.6.
 
