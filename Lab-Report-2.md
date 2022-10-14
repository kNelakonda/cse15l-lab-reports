# Lab Report 2
## Servers and Bugs

### My Code for the Simplest Search Engine
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
* ![Image](/Images/SearchExample1.png)
In the picture above, my program is adding the word "home" to a HashSet that keeps track of all the words. The code goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/add", and then url gets the Query and splits it up, and taking only the word after the "=" and adding it to the HashSet "words", which is a field. In this case the word is "home".

* ![Image](/Images/SearchExample2.png)
In the picture above, my program is adding the word "homerSimpson" to a HashSet that keeps track of all the words. The code goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/add", and then url gets the Query and splits it up, and taking only the word after the "=" and adding it to the HashSet "words", which is a field. In this case the word is "homerSimpson".

* ![Image](/Images/SearchExample3.png)
In the picture above, my program goes through the main method of SearchEngine, then the HandleRequest method. The HandleRequest method takes in the URL as an argument, and has a String to grab only the path. In this case, the path is "/search". After this, the program goes through each element of "words" and check if it contains whatever the second value of the query was. In this case, the search query was "me", so both "home" and "homerSimpson" were printed out, separated by commas. This was prefaced by "the search results are: ".