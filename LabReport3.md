Part 1: 

A failure-inducing input for the buggy program, as a JUnit test and any associated code 
- JUnit test that results in failure

```
@Test
public void testReversed() {
  int[] input1 = {1, 2, 3};
  assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
  assertArrayEquals(new int[]{1, 2, 3}, input1);
}
```
<br>
- The method (associated code)

```
static int[] reversed(int[] arr) {
  int[] newArray = new int[arr.length];
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = newArray[arr.length - i - 1];
  }
  return arr;
}
```

<br>
An input that doesn't induce a failure, as a JUnit test and any associated code  <br>
- JUnit test that passes

```
@Test
 public void testReversed2() {
   int[] input1 = { };
   assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
 }
```
- The method (associated code)

```
static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     arr[i] = newArray[arr.length - i - 1];
   }
   return arr;
 }
```

<br>
The symptom, as the output of running the tests


  
The bug, as the before-and-after code change required to fix it (changed the fourth and sixth lines in the code block)
- Before
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```
- After
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```


<br>
Why does this fix the issue?  

The error was that in the line `arr[i] = newArray[arr.length - i - 1];`, elements from `newArray` are being assigned to `arr`. 
This means that `arr` is being modified instead of `newArray`. However, we want to do the opposite - we want to reverse the 
elements of `arr` and store them in `newArray`. In addition, we had initialized `newArray` with default values (0), and its 
elements are not modified in the loop. As a result, `arr` is also just being assigned zeros. After we correct the line to 
`newArray[i] = arr[arr.length - i - 1];` , the reversed elements of `arr` are correctly assigned to `newArray`. In addition, 
changing the return statement to `return newArray;` now returns the correct reversed array.



Part 2:  

4 interesting options to use the `find` command:  

  

Option 1:` -mtime`  

https://www.redhat.com/sysadmin/linux-find-command  

This command helps find files by age/time of modification.   

Example 1: Within the past 3 days  

Input
`find . -mtime -3`  

Output
```
/~$llabus 19B 2024.docx
./Syllabus 19B 2024.docx
./.DS_Store
./Week5_B4_Lecture.pdf
./Syllabus_BICD100_WI24.pdf
./~$Updated WA2 W24 data.xlsx
./Bild 4 - Individual Writing Assignment .odt
./Writing2 Instructions.pdf
./P3.3.pages
./Bild 4 - Individual Writing Assignment .pdf
./Bild 4 - Individual Writing Assignment -2.pdf
./Updated WA2 W24 data.xlsx
./Lecture 7 Section B.pdf
```
Example 2: Exactly 1 day ago  

Input
`find . -mtime 1`   

Output
```
./~$llabus 19B 2024.docx
./Syllabus 19B 2024.docx
./Week5_B4_Lecture.pdf
./Bild 4 - Individual Writing Assignment .odt
./Writing2 Instructions.pdf
./Bild 4 - Individual Writing Assignment .pdf
./Bild 4 - Individual Writing Assignment -2.pdf
./Lecture 7 Section B.pdf
```


  <br>
Option 2: `-name:`  

Helps find files/directories with a specific name   

Example 1: Finding a file  

Input: `find ./Downloads -name "P3.3.docx"`  

Output: `./Downloads/P3.3.docx`  

Example 2: Finding a directory  

Input: `find ./Downloads -name "wavelet"`   

Output: `./Downloads/wavelet`  

  

<br>
Option 3:` -type`  

Helps specify whether we want to find a file or a directory  

Example 1: Finding a file  

Input: `find ./Downloads/week1Discussion-main-2 -type f`  

Output:
```
./Downloads/week1Discussion-main-2/DemoArrayImpl.java
./Downloads/week1Discussion-main-2/.DS_Store
./Downloads/week1Discussion-main-2/DemoArray.java
./Downloads/week1Discussion-main-2/libs/junit-4.12.jar
./Downloads/week1Discussion-main-2/libs/hamcrest-core-1.3.jar
./Downloads/week1Discussion-main-2/README.md
./Downloads/week1Discussion-main-2/DemoArrayImplTester.java 
```

Example 2: Finding a directory   

Input: `find ./Downloads/week1Discussion-main-2 -type d`  

Output: 
```
./Downloads/week1Discussion-main-2
./Downloads/week1Discussion-main-2/libs
```


  <br>
Option 4:` -empty`.  

 https://www.redhat.com/sysadmin/linux-find-command  
 
Helps find empty files (can help declutter)  

Finding empty files
Input
`find ./Documents -empty`  

Output
```
./Documents/.localized
./Documents/GitHub/lab3/.git/objects/info
./Documents/GitHub/lab3/.git/refs/tags
./Documents/GitHub/lab3/.git/branches
./Documents/MATLAB
```
When there's no empty files in the specified directory  

Input: `find ./Downloads/week1Discussion-main -empty`  

Output: No output (because no empty file found)  


<br>
<br>
ChatGPT Usage  

Input: Consider the commands find. Find 4 interesting command-line options or alternate ways to use the command. 
For example, we saw the -name option for find in class. For each of those options, give 2 examples of using
it on files and directories from ./technical.  

<br>
Output:
The `find` command is a powerful tool for searching files and directories recursively in a file system. 
Here are four interesting command-line options for the `find` command along with examples of their usage 
on files and directories from the `./technical` directory:

1. `-type` option:
   This option allows you to specify the type of the file to be searched.

   Example 1: Find all regular files in the `./technical` directory.
   ```
   find ./technical -type f
   ```

   Example 2: Find all directories in the `./technical` directory.
   ```
   find ./technical -type d
   ```

2. `-name` option:
   This option allows you to search for files or directories with a specific name pattern.

   Example 1: Find files named `README.md` in the `./technical` directory.
   ```
   find ./technical -name "README.md"
   ```

   Example 2: Find directories named `images` in the `./technical` directory.
   ```
   find ./technical -type d -name "images"
   ```

3. `-exec` option:
   This option allows you to execute a command on the files found by `find`.

   Example 1: Print the full path of all .txt files found in the `./technical` directory.
   ```
   find ./technical -type f -name "*.txt" -exec ls -l {} \;
   ```

   Example 2: Delete all `.bak` files found in the `./technical` directory.
   ```
   find ./technical -type f -name "*.bak" -exec rm {} \;
   ```

4. `-size` option:
   This option allows you to search for files based on their size.

   Example 1: Find files larger than 1MB in the `./technical` directory.
   ```
   find ./technical -type f -size +1M
   ```

   Example 2: Find files smaller than 100KB in the `./technical` directory.
   ```
   find ./technical -type f -size -100k
   ```

<br>
How I used and changed the output:   

I ended up using the first two options(-type and -name) that ChatGPT gave me, and tried to use the examples it
 gave me to create my own examples by experimenting with the command line on my own laptop. 
