# Lab Report 2 # 

## Part 1 ##
![Image](politz.png)

* In this screenshot, the handleRequest and main method are called. 
* The relevant argument to the handle request method is a URL of URI type. In this example the url is: 0-0-0-0-4001-a08i4vhucdejm6p9i8njpshb48.us.edusercontent.com/add-message?s=Hello&user=jpolitz. The Handler class also contains an ArrayList field of all message inputs to track previously added inputs. The ArrayList stores "jpolitz: Hello".
The main method also passes through command line arguments that we use to activate our server and run the handleRequest method.
* The ArrayList field originally stored nothing, but after running this path it was upated to store "jpolitz: Hello".

![Image](yash.png)

* In this screenshot, the handleRequest and main method are called. 
* The relevant argument to the handle request method is a URL of URI type. In this example the url is: 0-0-0-0-4001-a08i4vhucdejm6p9i8njpshb48.us.edusercontent.com/add-message?s=How%20are%20you&user=yash. The Handler class also contains an ArrayList field of all message inputs to track previously added inputs. The ArrayList stores "jpolitz: Hello"
and "yash: How are you". The main method also passes through command line arguments that we use to activate our server and run the handleRequest method.
* The ArrayList field originally stored "jpolitz: Hello" after the first command, and now it was updated to store "jpolitz: Hello" and "yash: How are you".

```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    ArrayList<String> messages = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return "Please enter a message";
        } else if(url.getPath().equals("/add-message")) {
            String[] parameters = url.getQuery().split("&");
            String user = null; 
            String message = null; 

            for(String parameter: parameters) {
                String[] key = parameter.split("=");
                if(key.length == 2) {
                    if(key[0].equals("user")) {
                        user = key[1];
                    } else if(key[0].equals("s")) {
                        message = key[1];
                    }
                }
            }

            if (user != null && message != null) {
                messages.add(String.format("%s: %s", user, message));
                StringBuilder result = new StringBuilder();
                for (String m : messages) {
                    result.append(m).append("\n");
                }
                return result.toString();
            } else {
                return "Invalid parameters!";
            }
        } else {
            return "404 Not Found!";
        }
    }
}

class ChatServer {
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

## Part 2 ##

1. The absolute path to the private key for your SSH key for logging into `ieng6`
![Image](private.png)

2. The absolute path to the public key for your SSH key for logging into `ieng6`
![Image](public.png)

3. A terminal interaction where you log into your `ieng6` account without being asked for a password.
![Image](nopassword.png)

## Part 3 ##
