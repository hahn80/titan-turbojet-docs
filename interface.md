# Interface to interact with TurboJet

## The main interaction:

We use Java and Python to make calls in between these two.

## Run Python as the server side
```sh
python turbojet.py
```

## Run Java JVM as client side

Check the TurboJet.java and Main.java to see the detail source.
The only thing we need to change when using the interaction via Java is the following lines
```java
          String fileName = "kfeatures.json";
          String service = "transformers";
```
The turbojet package has two main services: *transformers* and *converters*. Change the *service* if you want to switch in between these two.
For example:
```java
            String fileName = "linreg.json";
            String service = "converters";
```

To see the options and all posible functionalities, please check [transformers](transformers.md) and [converters](converters.md).
