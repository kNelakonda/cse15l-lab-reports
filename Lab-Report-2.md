# Lab Report 2 (The week of Lab 3)
## Servers and Bugs

### Part 1: My Code for the Simplest Search Engine
If the path is "/add", then it will add whatever is in the query to a HashSet of Strings, and if the path is "/search", then it will return all the strings that contain whatever is in the query.
```
import java.io.IOException;
import java.net.URI;
import java.util.HashSet;
class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    HashSet<String> words = new HashSet<String>();
    public String handleRequest(URI url) {
        String path = url.getPath();
        if(path.contains("/add")){
            String[] word = url.getQuery().split("=");
            words.add(word[1]);
            return "Word added";
        } else if (path.contains("/search")){
            String[] search = url.getQuery().split("=");
            String results = "the search results are: ";
            for (String i: words){
                if(i.contains(search[1])){
                    results = results.concat(i);
                    results = results.concat(", ");
                }
            }
            results = results.substring(0, results.length()-2);
            return results;
        } else {
            return "404 not found";
        }

    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

Here are some of the pictures of my program in action and what is happening in them:
![Image](/Images/SearchExample1.png)
* In the picture above, my program is adding the word "home" to a HashSet that keeps track of all the words. The code goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/add", and then url gets the Query and splits it up, and taking only the word after the "=" and adding it to the HashSet "words", which is a field. In this case the word is "home".

![Image](/Images/SearchExample2.png)
* In the picture above, my program is adding the word "homerSimpson" to a HashSet that keeps track of all the words. The code goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/add", and then url gets the Query and splits it up, and taking only the word after the "=" and adding it to the HashSet "words", which is a field. In this case the word is "homerSimpson".

![Image](/Images/SearchExample3.png)
* In the picture above, my program goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/search". After this, the program goes through each element of "words" and check if it contains whatever the second value of the query was. In this case, the search query was "me", so both "home" and "homerSimpson" were printed out, separated by commas. This was prefaced by "the search results are: ".

### Part 2: The Bugs from Lab 3

**A Bug From ListExamples**
Below is the test I ran for the merge method in ListExamples.java:

```
import static org.junit.Assert.assertArrayEquals;

import java.util.ArrayList;
import java.util.List;

import org.junit.Test;

public class ListTests{

    @Test
    public void mergeTest(){
        List<String> list1 = new ArrayList<String>();
        List<String> list2 = new ArrayList<String>();
        list1.add("brownies");
        list1.add("cakes");
        list1.add("cookies");
        list2.add("broccoli");
        list2.add("carrots");
        list2.add("spinach");
        List<String> list3 = ListExamples.merge(list1, list2);
        String[] expected = {"broccoli", "brownies","cakes","carrots","cookies","spinach"};
        assertArrayEquals(expected, list3.toArray());
    }
}
```
Below is a symptom of the test, the output in the terminal. There was an infinite loop.:

```
JUnit version 4.13.2
.E
Time: 16.613
There was 1 failure:
1) mergeTest(ListTests)
java.lang.OutOfMemoryError: Java heap space
        at java.base/java.util.Arrays.copyOf(Arrays.java:3512)
        at java.base/java.util.Arrays.copyOf(Arrays.java:3481)
        at java.base/java.util.ArrayList.grow(ArrayList.java:237)
        at java.base/java.util.ArrayList.grow(ArrayList.java:244)
        at java.base/java.util.ArrayList.add(ArrayList.java:454)
        at java.base/java.util.ArrayList.add(ArrayList.java:467)
        at ListExamples.merge(ListExamples.java:48)
        at ListTests.mergeTest(ListTests.java:21)
        at java.base/java.lang.invoke.LambdaForm$DMH/0x0000000801012400.invokeVirtual(LambdaForm$DMH)
        at java.base/java.lang.invoke.LambdaForm$MH/0x0000000801013000.invoke(LambdaForm$MH)
        at java.base/java.lang.invoke.Invokers$Holder.invokeExact_MT(Invokers$Holder)

FAILURES!!!
Tests run: 1,  Failures: 1
```

The bug that needed fixing was that the last loop of the method incremented the wrong index. index2 should be incremented in the last while loop instead of index1
```
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
    }
    return result;
  }
```
Above is the fix. kndex2 was always less that list2.size, since index1 was being incremented. The result was an infinite loop and you see the OutOfMemoryError.

**A Bug from 
