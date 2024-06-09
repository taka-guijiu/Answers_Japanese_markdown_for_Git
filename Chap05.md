Chapter 5
1. file.txt will appear in the “Unstaged Changes” text pane.
2. git citool is git gui, with the argument citool (i.e. git gui citool). For more details, type git
gui --help, and look at the COMMANDS section.
3. file.txt will appear in the “Staged Changes” text pane.
4. Following the instructions, you should see file.txt appear in both windows (see figure 1).
5. I’m afraid this is a bit of a trick question. The exercise has you adding a change (with git add), then
adding another change on top of the same file. However, the instructions actually overwrite the first
change with the second!
The best way to save both is to run git commit. This will save the first version of the file. Then git
add and git commit on the second change. This saves the second version of the file.
6. The chapter has us getting to the gitk program via git gui. It may be somewhat of a surprise that
you can get to the gitk without git gui, by typing gitk by itself.
gitk has no --help switch, but you can read about the command line arguments you can pass into it
by typing git help gitk.
2
Figure 1: file.txt in both places
3
