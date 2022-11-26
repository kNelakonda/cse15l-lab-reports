# Lab Report 5 (The week of Lab 8)
## Creating an Auto-grader Bash Script

**Our Auto-grader File (grade.sh)**
```
#Total student score out of 35
TOTAL_SCORE=0


function score_message(){

	printf "Student is given a score of $1/35 \n"
	exit 0
}

PREFIX=$PWD
CP=".:$PREFIX/lib/hamcrest-core-1.3.jar:$PREFIX/lib/junit-4.13.2.jar" # relative junit
TESTNAME="TestListExamples"
STUDENT_DIR="student-dir"

rm -rf $STUDENT_DIR 2> /dev/null

git clone $1 $STUDENT_DIR || exit 1 # names the repo $STUDENT_DIR 


set -o pipefail

INNARDS=$(ls ./$STUDENT_DIR)
# very bad case of a repo with just one subdirectory
if [[ -d ./$STUDENT_DIR/$INNARDS ]]; then
	STUDENT_DIR=$STUDENT_DIR/$INNARDS/
fi

cd ./$STUDENT_DIR/

# wrong name fix, one file expected though
#FIXME: I do not know how a file with multiple periods in filename may behave
badname=$(ls | cut -d '.' -f 1)
if [[ $badname.java != ListExamples.java ]]; then
	sed -i "s/${badname}/ListExamples/g" $badname.java
	mv $badname.java ListExamples.java > /dev/null
fi

cp $PREFIX/$TESTNAME.java ./


printf "<<-- Starting Compilation -->>\n"

javac -cp $CP *.java | tee output.txt

# check if their code compiled without errors
if [[ $? -ne 0 ]]; then
	
	echo compilation error
	score_message $TOTAL_SCORE

fi

# awarding them points for compiling
TOTAL_SCORE=$((TOTAL_SCORE + 5))


printf "<<-- Running Java -->>\n" 

java -cp $CP org.junit.runner.JUnitCore $TESTNAME | tee -a output.txt

# check if their code runs tests
if [[ $? -eq 0 ]]; then
	
	# awarding them full points
	TOTAL_SCORE=$((TOTAL_SCORE + 30))
else

	t=$(grep -i "Tests run" output.txt)
	tests=$(echo $t | cut -d ' ' -f 3 | cut -d ',' -f 1)
	failed=$(echo $t | cut -d ' ' -f 5)

	echo $((tests - failed))/$tests tests passed

	TOTAL_SCORE=$((TOTAL_SCORE + 15*(tests - failed))) # 15 is just the point per test in this example
fi

# Give a final score for the students
score_message $TOTAL_SCORE
```

### Examples of grade.sh output on a site

**Example 1: a completely correct submission**
![Image](/Images/correct-submission.png)

**Example 2: the first example, from lab 3**
![Image](/Images/original-from-lab-3.png)

**Example 3: a syntax error, a missing semicolon**
![Image](/Images/missing-semicolon.png)

### Tracing the script with Example 3

The beginning of the bash script starts with a lot of variables initializations and the creation of a function. `TOTAL_SCORE` is set to 0, which is used to determine the final score of the submission being graded. The function `scoring_message` will be used to output the message that shows what score the submission got. `PREFIX` will be used to output the path to the current working directory, `CP` will be the relative junit path, `TESTNAME` is the name of the TestListExamples class, and `STUDENT_DIR` is the name of the student-dir directory. In our example, these all are used, and these lines all all have return codes of 0.

The next command is line 19, or `rm -rf $STUDENT_DIR 2> /dev/null`. This is a recursive function to remove everything in the argument `$STUDENT_DIR`. This is actually a variable, and it was set to be `student-dir` a few lines above. The error code will be printed out in `/dev/null` if this command fails. For example 3, the return code is 0, and the program moves on with nothing printed out. 
The next command is the next line with text, which is `git clone $1 $STUDENT_DIR || exit 1 # names the repo $STUDENT_DIR`. The last part of this line, after the `#` is a comment. `git clone $1` will take in whatever the first argument was from the command, name it whatever is assigned to `STUDENT_DIR`, or will exit with exit code 1. In the example, 
The command following that is `set -o pipefail`, which will display any exit codes that are not 0 in the rest of the script. 
The if statement is to check if there is only one subdirectory. In this case there is none, so the script moves on, which is `cd ./$STUDENT_DIR/`, changing directories into `student-dir`. 

After the comments, the variable `badname` is initialized by finding the name of the .java file. This is done by first listing all the files (which should only be 1), using `ls`, then using `cut -d "." -f 1`, which will cute the entire file name, separateing each part with a period, and take only the first part. The if statement that follows will check if the file name is not the same as `ListExamples.java`, and change it to the correct name if true.

After confirming the file name is correct, we then copy the file to the current working directory.

Line 45, which is `printf "<<-- Starting Compilation -->>\n"` is notifying us that the files are going to be compiled, which is the command right after. This following line, `javac -cp $CP *.java | tee output.txt` will compile, replacing what was in `$CP` to access the library and compile, piping the standard output into output.txt, as designated by `| tee output.txt`. In Example 3, there is a compilation error, so there in the command line are statements telling us the error that occurred during compilation. In the if statement that follows, `if [[ $? -ne 0 ]]`, `$?` is not 0, so the commands that are in the if statement are executed. 

The first command in the if statement is `echo compilation error`, which will output to the command line "compilation error", letting us know that there is an error from compiling the java files. The following command calls a function that was written earlier in the script, called `score_message()` with the argument `$TOTAL_SCORE`, which is currently 0. The first command in `score_message()`, is `printf "Student is given a score of $1/35 \n"`, which uses the first argument from earlier, or `$TOTAL_SCORE`, as the score out of 35. The following line is `exit 0`, which will exit out of the bash script. None of the lines after line 55 in grade.sh are executed.

