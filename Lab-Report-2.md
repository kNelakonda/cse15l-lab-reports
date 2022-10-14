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
