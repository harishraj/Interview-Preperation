# Valid Number
> Validate if a given string is numeric.
>
> Example
>
> "0" => true
>
> " 0.1 " => true
>
> "abc" => false
>
> "1 a" => false
>
> "2e10" => true

## Method 1: Parsing
### Analysis
Before starting, should list all possbile cases that are valid. Then simply pasing the string.

### Corner Case
1. . is invalid, but 1. and .1 is valid
2. .e1 is invalid, however, 1.e1 and .1e1 is valid. 
3. do not allow dot after e, for example 1e1.1 is invalid 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public boolean isNumber(String s) {
    if (s == null || s.length() == 0) {
      return false;
    }
    int start = 0;
    int end = s.length() - 1;
    // skip preceding spaces
    while (start <= end && s.charAt(start) == ' ') {
      start++;
    }
    // skip trailing spaces
    while (start <= end && s.charAt(end) == ' ') {
      end--;
    }
    if (start > end) {   // all white spaces string
      return false;
    }
    // preceding '+'/'-'
    if (s.charAt(start) == '+' || s.charAt(start) == '-') {
      start++;
    }
    boolean hasNum = false;
    boolean hasExp = false;
    boolean hasDot = false;
    while (start <= end) {
      char c = s.charAt(start++);
      if (Character.isDigit(c)) {
        hasNum = true;
      } else if (c == '.') {  // .3 and 3. are valid
        if (hasDot || hasExp) {  // don't allow '.' after 'e'
          return false;
        }
        hasDot = true;
      } else if (c == 'e') {  // e3 is invalid, must have digit before 'e'
        if (hasExp || !hasNum) {
          return false;
        }
        hasNum = false;  // must have digits after e
        hasExp = true;
      } else if (c == '+' || c == '-') {  // must follow 'e'
        if (s.charAt(start - 2) != 'e') {
          return false;
        }
      } else {  // cannot have other characters, including spaces
        return false;
      }
    }
    return hasNum;
  }
}
```


## Method 2: DFA (too complicated to build DFA)
### Analysis
We should consider every case very carefully and discuss with the interviewer. Then we can construct a DFA to parse the string. 

<img src="https://lh4.googleusercontent.com/-ExeKuR_1yFc/VJIaOEWnb6I/AAAAAAAAAiY/IeC8oP3qMEk/w917-h688-no/IMG_1465.jpg)" width="500px"/>

### Code
#### Java
```java
public class Solution {
  public boolean isNumber(String s) {
    if (s == null || s == "") {
      return false;
    }
    s = s.trim();
    int state = 0;
    for (int i = 0; i < s.length(); ++i) {
      char c = s.charAt(i);
      if (c >= '0' && c <= '9') {
        if (state <= 2) {
          state = 2; 
        } else if (state <= 4) {
          state = 4;
        } else {
          state = 7;
        }
      } else if (c == '+' || c == '-') {
        if (state == 0 || state == 5) {
          ++state;
        } else {
          return false;
        }
      } else if (c == '.') {
        if (state <= 1) {
          state = 3;
        } else if (state == 2) {
          state = 4;
        } else {
          return false;
        }
      } else if (c == 'e') {
        if (state == 2 || state == 4) {
          state = 5;
        } else {
          return false;
        }
      } else {
        return false;
      }
    }
    if (state == 2 || state == 4 || state == 7) {
      return true;
    } else {
      return false;
    }
  }
}
```

