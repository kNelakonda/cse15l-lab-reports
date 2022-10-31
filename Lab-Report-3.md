# Lab Report 3 (The week of the Skills Demo)
## Researching Commands
### News ways to use the *find* command on the command line

#### 1. -size
One interesting way I was able to use find was to only find files of a certain size, or within the boundaries of a certain size. For example, the following searches within the current working directory all the files that are less than 10 kilobytes
```
$ find . -size -10k
```
The `.` specifies the directory, the `-size` option tells us we want to find files of a certain size, and `-10k` means everything less than 10 kilobytes, with `-` meaning less than, `10` being the size, and `k` being the size. If we want to check for larger files, the following letters are for other sized files:
c - bytes
k - kilobytes
M - megabytes
G - gigabytes
Here is the output when we type in the command to search for all the files within ./biomed that are less than 10 kilobytes:
```
$ find ./biomed -size -10k
./biomed/1472-6769-1-4.txt
./biomed/1471-2490-3-2.txt
./biomed/1471-2334-3-13.txt
```
Let's see if there are any larger files in ./biomed, maybe between 10 and 15 kilobytes:
```
$ find ./biomed -size +10k -size -15k
./biomed/1472-6750-3-11.txt
./biomed/bcr635.txt
./biomed/1472-6807-2-5.txt
./biomed/1471-2350-4-4.txt
./biomed/1472-6769-1-3.txt
./biomed/1477-7819-1-10.txt
./biomed/1471-2229-3-3.txt
./biomed/1471-230X-1-6.txt
./biomed/ar383.txt
./biomed/1472-6882-2-5.txt
./biomed/cc713.txt
./biomed/1471-5945-3-3.txt
./biomed/1471-2105-3-24.txt
./biomed/1471-2350-2-12.txt
./biomed/1471-2148-2-5.txt
./biomed/cc973.txt
./biomed/1471-213X-2-8.txt
./biomed/1471-2180-2-29.txt
./biomed/cc1547.txt
./biomed/1471-2407-2-9.txt
./biomed/1471-2156-2-8.txt
./biomed/1471-2156-3-22.txt
./biomed/1476-4598-1-5.txt
./biomed/1471-2148-1-6.txt
./biomed/1471-2210-1-2.txt
./biomed/1471-230X-2-17.txt
./biomed/1471-2350-2-8.txt
./biomed/1471-2121-2-3.txt
```
I can put `-size` twice as options and it will specify the range between the two.
Let's combine this with the `-name` option, and see if we can find any within all of `./technical` that are smaller than 1 kilobyte, but only .txt files:
```
$ find . -size -1k -name "*.txt"  
./plos/pmed.0020191.txt
./plos/pmed.0020226.txt
```
This recursively found all the .txt files that are less than 1 kilobyte, and prints out the path starting from the working directory.
#### 2. -type
If I want, I can find all the subdirectories in my working directory to see how many folders there are and what the names of them are. `-type` is the option, and the argument is "d", for directories. I can also do "f" for all the files.
Let's try this on the command line:
```
$ find . -type d           
.
./government
./government/About_LSC
./government/Env_Prot_Agen
./government/Alcohol_Problems
./government/Gen_Account_Office
./government/Post_Rate_Comm
./government/Media
./plos
./biomed
./911report
```
This printed out all the subdirectories that are within `/technical`, but none of the files, since I specified d

There are a lot within `/government`, so let's boil it down to only the directories within `/government`:
```
$ find ./government -type d        
./government
./government/About_LSC
./government/Env_Prot_Agen
./government/Alcohol_Problems
./government/Gen_Account_Office
./government/Post_Rate_Comm
./government/Media
```

That is still a lot, but I only want to find the ones that start with the letter "A":
```
$ find . -type d -name "A*"
./government/About_LSC
./government/Alcohol_Problems
```
To find the directories that start with the letter "A", we add the option `-name` and the name of what we are looking for. We need to make sure we capitalize A in the command, which is why A is capitalized. `-name` is case sensitive. There is, however, a way to find things without case sensitivity, which leads to another option...

#### 3. -iname
I am trying to find some files that were written in September, and they must have the word "September" in the name, but I'm not sure if it was capitalized. Luckily, there's an option for finding files regardless of capitalization:
```
$ find ./government -iname "sept*.txt" 
./government/Gen_Account_Office/Sept27-2002_d02966.txt
./government/Gen_Account_Office/Sept14-2002_d011070.txt
```
There are two files which have "Sept" in the file name, and the entire path from the working directory is listed. The "*" allowed for autocompleting the rest of the name of the .txt file.

There's another government file that has to do with Ontario, so let's try finding it:
```
$ find ./government -name "Ontario*.txt" 
$ 
```
Using the `-iname` option is case insensitive as opposed to `-name` which is case sensitive.
Looks like more of "Ontario" was capitalized, so let's use `-inname` so our search is case insensitive:
```
$ find ./government -iname "Ontario*.txt"
./government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
```

I'm going to go ahead and delete that file now:
```
$ find ./government -iname "Ontario*.txt" -exec rm {} \;
$ find ./government -iname "Ontario*.txt"                  
$
```
The last line confirms that I cannot find it anymore, and it is now deleted. I use the `-exec` to execute a command, which I choose as `rm` to delete files. I combined the `-iname` option along with the `-exec` option to not only find the file, but delete it as well.

There are plenty of useful applications for find, and it might even be easier to find some files and directories over terminal than through the Finder if you know the proper commands.
