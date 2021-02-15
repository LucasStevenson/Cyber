# Javavavaa

> Points: 125

> Category: Reverse Engineering

## Description
Could you help me find the epic flag for this fine challenge?

**Note**: please put the fine flag wrapper around your answer

## Attachments
https://fdownl.ga/9BCD466775

## Solution

This is the important part of the program

```java
...
int intint = 673; 
int intin = 547; 
String variable_name = ""; 

//Perform epic operation with epic values ~~with epic fallback~~
for (int i = 0; i < xorVars.length; i++) 
{ 
    if (xorVars[i] != ' ') { 
	variable_name += (char) ((((intint * (xorVars[i] - 'a')) + intin) % 26) + 'a'); 
    } else { 
	variable_name += xorVars[i]; 
    } 
}
...
```

Put the encoding logic into a function called ``encode`` and pass ascii values that will most likely be in the flag (60-126) through it. Save the original and encoded values to their own strings.

For each character in an encoded message, find it's index (let's call it **i**) in the **encoded values** string. The "decoded" character is the character in the **ascii original values** string at position **i**. 


```java
import java.util.*;
class Main {
    public static char encode(char c) {
        return (char) ((((673 * (c - 'a')) + 547) % 26) + 'a');
    }

    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);

        String asciiValString = "";
        String encodedValString = "";
        for (int i = 60; i <= 126; i++) {
            char asciiVal = (char) i;
            asciiValString += asciiVal;
            encodedValString += encode(asciiVal);
        }
        //System.out.println(asciiValString);
        //System.out.println();
        //System.out.println(encodedValString);

        System.out.print("Enter encoded value: ");
        String encStr = sc.nextLine();

        for (int i = 0; i < encStr.length(); i++) {
            System.out.print(asciiValString.charAt(encodedValString.indexOf(encStr.charAt(i))));
        }
        System.out.println();
        sc.close();
    }
}
```

### Flag
> ictf{ionlyliketheepicflags}

