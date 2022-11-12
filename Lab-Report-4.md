# Lab Report 4 (The week of Lab 7)
## More on Vim

#### Part 1: How I did a task from Week 6 with vim in as few keystrokes as possible
**Task: Change the main method to take in a command-line argument**
Below are the keystrokes:
`vim D<Tab><Enter>/tec<Enter>nce<Esc>hhxxxxxiargs[1]<Esc>:wq`

Here are some of the noteworthy parts:
When typing `/tec<Enter>` it got me to the first occurence of the `tecn` characters in the file, and then `n` right after to get to the next occurence of it, which is why I am down at the bottom of the file, and you can see the first ocurrence at the top:
![Image](/Images/tec-enter-example.png)

After typing `ce`, I deleted the entire word, and was put into insert mode:
![Image](/Images/deleted-technical.png)

Hitting `<Esc>hh` moved my cursor two characters to the left (as seen by the first pic), and then typing `xxxxx` deleted 5 characters in a row that I was hovering over (as shown by the second pic):
![Image](/Images/hh-cursor-left.png)

![Image](/Images/xxxxx.png)

Pressing `i` put me in insert mode, followed by `args[1]` to type where my cursor was:
![Image](/Images/args1.png)

I then typed `<Esc>` to exit insert mode, and then `:wq` to save and quit:
![Image](/Images/save-and-quit.png)

#### Part 2: Performing edits locally versus remotely
**Task 1: Make the edits, copy the file remotely, and run the file**
Time: 3:00

**Task 2: Make the edits remotely and run the files remotely**
Time: 2:00

**Question 1: Which of these two styles would you prefer using if you had to work on a program that you were running remotely, and why?**
I think for the most part, if I had to edit a lot of files back and forth for a program I am running remotely, I would definitely prefer making edits over vim instead of making edits through a local IDE such as Visual Studio and then copying them to the remote machine. This takes time, a lot of extra commands, and can get confusing due to how many moving pieces there are. Learning vim definitely would make this process more efficient. Only benefits of working locally are that the IDE can catch some errors before compilation time so I wouldn't be compiling and running files that clearly have errors, that I happened to have missed.

**Question 2: What about the project or task might factor into your decision one way or another? (If nothing would affect your decision, say so and why!)**
The more files and that need to be ran remotely, the more likely I would be to use vim and edit files with it. On occasion I may find myself editing some files locally, but that is only if I am unsure about some small things. 