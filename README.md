# File Reverser Lab Guide

## Learning Objectives
- Basic Java file I/O operations
- Error handling with try-catch blocks
- Resource management with try-with-resources
- Working with buffered readers and writers
- Using ArrayLists and the Collections framework

## Step 1: Hello World Foundation
### Concepts Covered
- Basic Java program structure
- Program entry point
- System output

```java
public class FileReverser {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

**Exercise**: Run this program to verify everything is working. Change ``World`` to your name and run again. 

## Step 2: Basic File Reading
### Concepts Covered
- FileReader introduction
- Reading characters
- Basic error handling
- Manual resource closing

```java
import java.io.FileReader;
import java.io.IOException;

public class FileReverser {
    public static void main(String[] args) {
        FileReader reader = null;
        try {
            reader = new FileReader("input.txt");
            int character;
            while ((character = reader.read()) != -1) {
                System.out.print((char)character);
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                System.out.println("Error closing reader: " + e.getMessage());
            }
        }
    }
}
```

**Extra**: Test this program on different contents inside the ``input.txt`` file. 

## Step 3: Basic File Writing
### Concepts Covered
- FileWriter introduction
- Writing characters
- Managing multiple resources
- Error handling for multiple operations

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileReverser {
    public static void main(String[] args) {
        FileReader reader = null;
        FileWriter writer = null;
        try {
            reader = new FileReader("input.txt");
            writer = new FileWriter("output.txt");
            
            int character;
            while ((character = reader.read()) != -1) {
                writer.write(character);
            }
        } catch (IOException e) {
            System.out.println("Error with file operations: " + e.getMessage());
        } finally {
            try {
                if (reader != null) reader.close();
                if (writer != null) writer.close();
            } catch (IOException e) {
                System.out.println("Error closing resources: " + e.getMessage());
            }
        }
    }
}
```

**Extra**: Modify the program to convert all text to uppercase while copying.

## Step 4: Implementing Try-with-Resources
### Concepts Covered
- Try-with-resources syntax
- AutoCloseable interface
- Improved resource management
- Cleaner code structure

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileReverser {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("input.txt");
             FileWriter writer = new FileWriter("output.txt")) {
            
            int character;
            while ((character = reader.read()) != -1) {
                writer.write(character);
            }
            
        } catch (IOException e) {
            System.out.println("Error with file operations: " + e.getMessage());
        }
    }
}
```

**Extra**: Create a second output file that contains a character count summary.

## Step 5: Adding Buffering
### Concepts Covered
- Buffered readers and writers
- Reading by lines instead of characters
- Performance improvements
- Line-based operations

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileReverser {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("input.txt"));
             BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            
            String line;
            while ((line = reader.readLine()) != null) {
                writer.write(line);
                writer.newLine();
            }
            
        } catch (IOException e) {
            System.out.println("Error with file operations: " + e.getMessage());
        }
    }
}
```

## Step 6: Final Implementation with Reversal
### Concepts Covered
- ArrayList usage
- Collections framework
- Separating read and write operations
- Enhanced for loop

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;

public class FileReverser {
    public static void main(String[] args) {
        ArrayList<String> lines = new ArrayList<>();
        
        try (BufferedReader reader = new BufferedReader(new FileReader("input.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                lines.add(line);
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
            return;
        }
        
        Collections.reverse(lines);
        
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("reverse.txt"))) {
            for (String line : lines) {
                writer.write(line);
                writer.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }
}
```
