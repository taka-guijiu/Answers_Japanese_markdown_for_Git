Chapter 11
1. This exercise assumes that you have two directories: math, which is the initial Git repository, and
math.clone, which you created with git clone math math.clone.
To complete this exercise, make another copy of the math directory using cp -r math math.copy,
and then type git log --oneline --all and git branch --all in both the math.clone and the
math.copy directories (remember: these directories are Git repositories). The git branch output will
be different between the math.clone and math.copy directories.
% cp -r math math.copy
% cd math.copy
% git branch --all
* another_fix_branch
master
new_feature
% cd ../math.clone
% git branch --all
* another_fix_branch
remotes/origin/HEAD -> origin/another_fix_branch
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature
2. Similar to the above, you’ll be making a clone of the math repository, but this time with the current
branch of math as fixing_readme. (This branch was created back in Chapter 9. If you don’t have this
branch, reread section 9.2.2.) A session would look like this:
% cd math
% git checkout fixing_readme
Switched to branch 'fixing_readme'
% cd
% git clone math math.clone4
% cd math.clone4
% git branch
* fixing_readme
3. If you type git branch --all, you’ll see all the available branches, including the remote tracking
branches. The output might be this:
% git branch --all
* another_fix_branch
remotes/origin/HEAD -> origin/another_fix_branch
remotes/origin/another_fix_branch
remotes/origin/master
remotes/origin/new_feature
In section 11.1.3, you saw that you could type ‘new_feature’, leaving out the ‘remotes/origin’ prefix:
% git checkout new_feature
Branch new_feature set up to track remote branch new_feature from origin.
Switched to a new branch 'new_feature'
The consequences of this automated ‘set up’ will be discussed in section 13.1.2.
What if, by mistake, you specify the full name of the remote branch?
% git checkout remotes/origin/new_feature
Note: checking out 'remotes/origin/new_feature'.
21
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:
git checkout -b <new-branch-name>
HEAD is now at b99624d... Starting a second new file
This variation does not do the automatic set up, and you are in a detached HEAD situation, as described
in Chapter 8.
4. This exercise invites you to create a confusing situation: naming a local branch different from its
remote-tracking branch. There may be times when this will come in handy (perhaps if you need two
copies of a single branch, for example).
Your session might look like this:
% git branch --all
fixing_readme
* new_feature
remotes/origin/HEAD -> origin/fixing_readme
remotes/origin/another_fix_branch
remotes/origin/fixing_readme
remotes/origin/master
remotes/origin/new_feature
% git checkout -b my_other_fixing_readme remotes/origin/fixing_readme
Branch my_other_fixing_readme set up to track remote branch fixing_readme from origin.
Switched to a new branch 'my_other_fixing_readme'
Section 13.1.3 will describe the git remote -v show origin, which can help you keep track of which
remote-tracking branch goes with which local branch.
5. As far as I know, there is no limit to the number of branches that use a particular starting point
(starting commit).
6. The --origin switch in git clone allows you to specify a different name or label for the remote
location. A session that demonstrates --origin would look as follows:
% git clone --origin distant_planet math math.clone5
Cloning into 'math.clone5'...
done.
% cd math.clone5
% git branch --all
* fixing_readme
remotes/distant_planet/HEAD -> distant_planet/fixing_readme
remotes/distant_planet/another_fix_branch
remotes/distant_planet/fixing_readme
remotes/distant_planet/master
remotes/distant_planet/new_feature
22
