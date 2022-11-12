# Lab Report 3 (The week of Lab 7)
## More on Vim

#### Part 1: How I did a task from Week 6 with vim in as few keystrokes as possible
**Task: Change the main method to take in a command-line argument**
Below are the keystrokes:
`vim D<Tab><Enter>/tec<Enter>nce<Esc>hhxxxxxiargs[1]<Esc>:wq`

Here are some of the noteworthy parts:
When typing `/tec<Enter>` it got me to the first occurence of the `tecn` characters in the file, and then `n` right after to get to the next occurence of it, which is why I am down at the bottom of the file, and you can see the first ocurrence at the top:
![Image](/Images/%3Atec%20%3Center%3E%20example.png)

After typing `ce`, I deleted the entire word, and was put into insert mode:
![Image](/Images/deleted-technical.png)

Hitting `<Esc>hh` moved my cursor two characters to the left (as seen by the first pic), and then typing `xxxxx` deleted 5 characters in a row that I was hovering over (as shown by the second pic):
![Image](/Images/hh-cursor-left.png)

![Image](/Images/xxxxx.png)

Pressing `i` put me in insert mode, followed by `args[1]` to type where my cursor was:
![Image](/Images/args%5B1%5D.png)

I then typed `<Esc>` to exit insert mode, and then `:wq` to save and quit:
![Image](/Images/save-and-quit.png)

