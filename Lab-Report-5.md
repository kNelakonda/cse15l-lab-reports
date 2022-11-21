# Lab Report 5 (The week of Lab 8)
## Creating an Auto-grader Shell Script

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

