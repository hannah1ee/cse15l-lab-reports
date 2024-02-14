# Lab Report 03 | Bugs and Commands

## Part 1 | Bugs

### A failure-inducing input for the buggy program
```
@Test
public void testReverseInPlace_Failure() {
    int[] input1 = { 1, 2, 3, 4, 5 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 5, 4, 3, 2, 1 }, input1);
}
```
### An input that doesn't induce a failure
```
@Test 
	public void testReverseInPlace_NoFailure() {
    int[] input1 = { 5 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 5 }, input1);
	}
```

### The symptom, as the output of running the tests

![Image](lab03_3a.png)
- This output shows that the test for testReverseInPlace_NoFailure runs without a problem, which the test for testReverseInPlace_Failure failures. 

![Image](lab03_3b.png)
- This is the output when the test does not fail. This test means that the function returned 5 as expected. 

![Image](lab03_3c.png)
- This is the output when the test fails. This test means that the function returned returned 4 when it expected 2. 


### The bug, as the before-and-after code change required to fix it 

Before code change:
```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

After code change:
```
static void reverseInPlace(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n / 2; i++) {
        int temp = arr[i];
        arr[i] = arr[n - i - 1];
        arr[n - i - 1] = temp;
    }
}
```

### Briefly describe why the fix addresses the issue.

This fix addresses the issue by traversing only half of the array and swapping the elements at the current index with the corresponding element from the end of the array. This ensures that the array is correctly reversed in place.

## Part 2 | Researching Commands

I focused on the command `find`

### option 1. `-type` 
- this option makes it so that `find` searches for a specific type of file
- some example types of files that could be searched for include regular files, directories, symbolic links, etc.
- [Find - Linux man page](https://linux.die.net/man/1/find)
   - I found the `-type` option by looking through the Standards Conformance section, which outlines the various standards and specifications that `find` adheres to 

#### Example 1: finding all directories within ./technical

using command: 
```
find ./technical -type d
```

output: 
```
./technical
./technical/911report
./technical/biomed
```

This command searches for and lists all driectories within the ./technical directory, showing the paths of the directories founnd, including subdirectories if any. It is useful for identifying the directory structure within the specified path 


#### Example 2: finding all regular files within ./technical

using command: 
```
find ./technical -type f
```

output: 
```
./technical/911report/chapter-3.txt
./technical/911report/chapter-11.txt
./technical/911report/chapter-13.5.txt
 # more lines of output
./technical/biomed/1471-2121-2-10.txt
```

This command searches for and lists all regular files within the ./technical directory. The output are the paths of the regular files found, including any subdirectories. This command is useful for identifying all regular files within a specified directory and any of its subdirectories.

### option 2. `-name` 
- this option makes it so that `find` searches for files within a specific name or pattern
- [Find - Linux man page](https://linux.die.net/man/1/find) 
   - I found the `-name` option by looking through the Standards Conformance section, which outlines the various standards and specifications that `find` adheres to 

#### Example 1: finding files with "chapter" in the name within ./technical

using command: 
```
find ./technical -name "*chapter*"
```

output: 
```
./technical/911report/chapter-3.txt
./technical/911report/chapter-11.txt
./technical/911report/chapter-13.5.txt
 # more lines of output
./technical/911report/chapter-13.4.txt
```

This command searches for files within the ./technical directory that have "chapter" in their name. It outputs the paths of the files found, including subdirectories if any. This is useful for locating files that are chapters within reports.

#### Example 2: finding files with ".txt" at the end of the name within ./technical

using command: 
```
find ./technical -name "*.txt"
```

output: 
```
./technical/911report/chapter-3.txt
./technical/911report/chapter-11.txt
./technical/911report/chapter-13.5.txt
 # more lines of output
./technical/biomed/1471-2121-2-10.txt
```

This command searches for files within the ./technical directory that have ".txt" at the end of their name. It outputs the paths of the text files found, including any subdirectories. This is useful for locating files that are in the .txt format.

### option 3. `-size` 
- this option makes it so that `find` searches for files based on their size
- [Find - Linux man page](https://linux.die.net/man/1/find) 
   - I found the `-size` option by looking through the TESTS section, which outlines the available options for specifying conditions or characteristics to filter files or directories.

#### Example 1: finding files smaller than 100KB within ./technical

using command: 
```
find ./technical -type f -size -100k
```

output: 
```
./technical/911report/chapter-11.txt
./technical/911report/chapter-8.txt
./technical/911report/chapter-13.1.txt
 # more lines of code
./technical/biomed/1471-2121-2-10.txt
```

This command searches for regular files within the ./technical directory that are smaller than 100 kilobytes. It outputs the paths of the files found, including any subdirectories. This command is useful for identifying files that may be smaller in size that 100 kilobytes to take further action upon them.

#### Example 2: finding files larger than 200KB within ./technical

using command: 
```
find ./technical -type f -size +200k
```

output: 
```
./technical/911report/chapter-3.txt
./technical/911report/chapter-13.5.txt
./technical/911report/chapter-13.4.txt
```

This command searches for regular files within the ./technical directory that are larger than 200 kilobytes. It outputs the paths of the files found, including any subdirectories. This command is useful for identifying files that may be larger in size than 200 kilobytes to possibly avoid messing with them. 

### option 4. `-empty` 
- this option makes it so that `find` searches for empty files and directories 
- [Find - Linux man page](https://linux.die.net/man/1/find)
  - I found the `-empty` option by looking through the Environment Variables section, which outlines variables that can be set to customize aspects of how `find` operates.

#### Example 1: finding all empty files within ./technical

using command: 
```
find ./technical -type f -empty
```

output: 
```
./technical/biomed/ar408.txt
./technical/biomed/ar118.txt
./technical/biomed/1477-7827-1-23.txt
 # more lines of code
./technical/biomed/1475-9276-1-3.txt
```

This command searches for regular files within the ./technical directory that are empty. It outputs the paths of the empty files found, including any subdirectories. This is useful for identifying and managing empty files, which may be files that we wish to delete. 


#### Example 2: finding all empty directories within ./technical

using command: 
```
find ./technical -type d -empty
```

output: 
```
# no output because no empty directories
```

This command searches for directories within the ./technical directory that are empty. It outputs the paths of the empty directors found, which in this case are none. This is useful for identifying and managing empty directories, which may be directories that we can build upon or remove. 


