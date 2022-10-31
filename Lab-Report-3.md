# Lab Report 3 (The week of the Skills Demo)
## Researching Commands
### News ways to use the *find* command on the command line

1. One interesting way I was able to use find was to only find files of a certain size, or within the boundaries of a certain size.
For example, the following searches within the current working directory all the files that are less than 10 kilobytes
```
$ find . -size -10k
```
The "." specifies the directory, the "-size" option tells us we want to find files of a certain size, and "-10k" means everything less than 10 kilobytes, with "-" meaning less than, "10" being the size, and "k" being the size. If we want to check for larger files, the following letters are for other sized files:
* c - bytes
* k - kilobytes
* M - megabytes
* G - gigabytes

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
(Wow, that is a lot!)

Let's combine this with the -name option, and see if we can find any within all of ./technical that are smaller than 1 kilobyte, but only .txt files:
```
$ find . -size -1k -name "*.txt"  
./plos/pmed.0020191.txt
./plos/pmed.0020226.txt
```
(Oh, only two in ./plos)

