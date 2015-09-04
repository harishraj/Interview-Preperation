# Read N Characters Given Read4
### Source
1. [leetcode](https://leetcode.com/problems/read-n-characters-given-read4/)

> The API: int read4(char *buf) reads 4 characters at a time from a file.
>
> The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.
>
> By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.
> 
> Note:
>
> The read function will only be called once for each test case.

### Analysis
Simply read until eof all already read n characters.

### Corner Case:
1. When for examples four characters were read, but actually we only need to read 2 more characters.

### Complexity
Time: O(N)

Space: O(1)

### Code
```java
/* The read4 API is defined in the parent class Reader4.
    int read4(char[] buf); */

public class Solution extends Reader4 {
  /**
   * @param buf Destination buffer
   * @param n   Maximum number of characters to read
   * @return    The number of characters read
   */
  public int read(char[] buf, int n) {
    char[] buffer = new char[4];
    int totalBytesRead = 0;
    boolean eof = true;
    while (!eof && totalBytesRead < n) {
      int bytesRead = read4(buffer);
      if (bytesRead < 4) {
        eof = true;
      }
      int bytesNeeded = Math.min(bytesRead, n - totalBytesRead);
      for (int i = 0; i < bytesNeeded; i++) {
        buf[totalBytesRead + i] = buffer[i];
      }
      totalBytesRead += bytesNeeded;
    }
    return totalBytesRead;
  }
}
```
