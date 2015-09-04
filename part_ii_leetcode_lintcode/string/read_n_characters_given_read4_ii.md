# Read N Characters Given Read4 II - Call multiple times
### Source
1. [leetcode](https://leetcode.com/problems/read-n-characters-given-read4-ii-call-multiple-times/)

> The API: int read4(char *buf) reads 4 characters at a time from a file.
> 
> The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.
>
> By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.
>
> Note:
>
> The read function may be called multiple times.

### Analysis
The difference between [Read N Characters Given Read4](read_n_characters_given_read4.md) is that we need to save the status of the internal buffer if we haven't read all characters from it. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
/* The read4 API is defined in the parent class Reader4.
    int read4(char[] buf); */

public class Solution extends Reader4 {
  private char[] buffer = new char[4];
  private int offset = 0;
  private int charactersInBuffer = 0; 

  /**
   * @param buf Destination buffer
   * @param n   Maximum number of characters to read
   * @return    The number of characters read
   */
  public int read(char[] buf, int n) {
    int totalCharactersRead = 0;
    boolean eof = false;
    while (!eof && totalCharactersRead < n) {
      if (charactersInBuffer == 0) {
        charactersInBuffer = read4(buffer);
        eof = charactersInBuffer < 4;
      }
      int numCharactersUsed = Math.min(charactersInBuffer, n - totalCharactersRead);
      for (int i = 0; i < numCharactersUsed; i++) {
        buf[totalCharactersRead + i] = buffer[offset + i];
      }
      totalCharactersRead += numCharactersUsed;
      charactersInBuffer -= numCharactersUsed;
      offset = (offset + numCharactersUsed) % 4;
    }
    return totalCharactersRead;
  }
}
```
