Chapter 6
Ch 6.4.1
1. Instead of --staged, use --cached. See git diff --help.
2. git add -n
3. cat -n FILENAME will display the contents of FILENAME with line numbers.
4. git log --format=oneline --abbrev-commit
5. --all. Go figure!
Ch 6.4.2
To see the command to get rid of a change that you’ve added to Git via git add, use git status.
Ch 6.4.3
In your favorite editor, create a file readme.txt in the math directory. It can contain text or it can be empty.
Now type:
git add readme.txt
git commit -m "Adding readme.txt"
