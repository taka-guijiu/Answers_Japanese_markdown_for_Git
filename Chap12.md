Chapter 12
Ch 12.4.1
1. The output of git log --oneline --decorate is something like this.
% git log --oneline --decorate
4465c54 (HEAD, origin/master, origin/HEAD, master) A small update to readme.
6f6af16 Adding printf.
256d402 Adding two numbers.
23d3077 (origin/another_fix_branch) Renaming c and d.
80f5738 Removed a and b.
94abe13 Adding readme.txt
ef47d3f (tag: four_files_galore) Adding four empty files.
3847b0b Adding b variable.
2732d6a This is the second commit.
e7a974f This is the first commit.
2. 23d3077
3. Yes: four_files_galore.
4. The SHA1 IDs of math.github will be different from math.bob and math.clone. SHA1 IDs do not repeat
when generated on different files and servers (see section 8.1.1). When SHA1 IDs are generated for
commits, your specific computer and commit information is used in the generation of the SHA1 ID.
Because of this, there is no way these SHA1 IDs would ever be the same.
Ch 12.4.2
This exercise repeats sections 12.1.3 and 12.2. Your session shoud look similar to this:
% cd math.bob
% git remote
origin
% git remote add carol ../math.carol
% cd ../math.carol
% vim readme.txt
% git commit -a -m "Small update to readme.txt"
[master 4ec7d23] Small update to readme.txt
1 file changed, 1 insertion(+)
% cd ../math.bob
% git ls-remote carol
4ec7d2333b71eb7ed1f4c3e50e082d8a278414f0 HEAD
4ec7d2333b71eb7ed1f4c3e50e082d8a278414f0 refs/heads/master
ffe2dda78e503a618d87782e960e5b3e8371db89 refs/remotes/origin/HEAD
249644fc932f6e005a4eb2d705159b8eb8f21c75 refs/remotes/origin/another_fix_branch
ffe2dda78e503a618d87782e960e5b3e8371db89 refs/remotes/origin/master
03d04ea643f9344f814853cc670cc221882cd995 refs/remotes/origin/new_feature
2edbb3543e903400f39d9e19e516d321b3855e24 refs/tags/four_files_galore
411dc82706661c0b65664c0dd97fbd7c4ea5f9f3 refs/tags/four_files_galore^{}
The key is that the commit you make in math.carol (4ec7d23) is shown in the git ls-remote carol
command.
Ch 12.4.3
This exercise asks you to try out the other git remote commands. In the math.bob directory, your
experiments with the delete and update commands might look like this:
23
% cd math.bob
% git remote remove carol
% git remote
origin
% git remote add tester ../math.nonexist
% git remote -v show
origin /home/rick/gitbook/math.git (fetch)
origin /home/rick/gitbook/math.git (push)
tester ../math.nonexist (fetch)
tester ../math.nonexist (push)
% git remote set-url tester ../math.carol
% git remote -v show
origin /home/rick/gitbook/math.git (fetch)
origin /home/rick/gitbook/math.git (push)
tester ../math.carol (fetch)
tester ../math.carol (push)
The set-url subcommand of git remote seems to allow you to specify multiple URLs for a remote, by
employing the –add switch. For example:
% git remote set-url --add tester ../math.another
% git remote -v show
origin /home/rick/gitbook/math.git (fetch)
origin /home/rick/gitbook/math.git (push)
tester ../math.carol (fetch)
tester ../math.carol (push)
tester ../math.another (push)
This seems to be a way to use one remote name to push to multiple locations! I haven’t tested this, but I
could see where it might be useful.
Ch 12.4.4
What you should see if you try to clone to a directory that already exists using Git GUI is:
Figure 3: Git GUI won’t overwrite an existing directory
Once you make the clone in Git Gui, you can compare the SHA1 IDs with git log between the new clone
and your earlier math.github clone.
24
Ch 12.4.5
Yes, the SHA1 IDs are the same. Your session could look like this:
% git clone https://gitlab.com/rickumali/math math.gitlab
Cloning into 'math.gitlab'...
remote: Counting objects: 35, done.
remote: Compressing objects: 100% (23/23), done.
remote: Total 35 (delta 6), reused 0 (delta 0)
Unpacking objects: 100% (35/35), done.
Checking connectivity... done.
% cd math.gitlab
% git remote -v
origin https://gitlab.com/rickumali/math (fetch)
origin https://gitlab.com/rickumali/math (push)
% git log --oneline
4465c54 A small update to readme.
6f6af16 Adding printf.
256d402 Adding two numbers.
23d3077 Renaming c and d.
80f5738 Removed a and b.
94abe13 Adding readme.txt
ef47d3f Adding four empty files.
3847b0b Adding b variable.
2732d6a This is the second commit.
e7a974f This is the first commit.
% cd ../math.github
% git remote -v
origin https://github.com/rickumali/math.git (fetch)
origin https://github.com/rickumali/math.git (push)
% git log --oneline
4465c54 A small update to readme.
6f6af16 Adding printf.
256d402 Adding two numbers.
23d3077 Renaming c and d.
80f5738 Removed a and b.
94abe13 Adding readme.txt
ef47d3f Adding four empty files.
3847b0b Adding b variable.
2732d6a This is the second commit.
e7a974f This is the first commit.
