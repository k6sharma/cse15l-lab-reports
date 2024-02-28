Part 1: 

A failure-inducing input for the buggy program, as a JUnit test and any associated code 
- JUnit test that results in failure

`@Test
 public void testReversed() {  <br>
   int[] input1 = {1, 2, 3};  <br>
   assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));  <br>
   assertArrayEquals(new int[]{1, 2, 3}, input1);  <br>
 }`
- The method (associated code)
`static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     arr[i] = newArray[arr.length - i - 1];
   }
   return arr;
 }`

An input that doesn't induce a failure, as a JUnit test and any associated code 
- JUnit test that passes
`@Test
 public void testReversed2() {
   int[] input1 = { };
   assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
 }`
- The method (associated code)
`static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     arr[i] = newArray[arr.length - i - 1];
   }
   return arr;
 }`

The symptom, as the output of running the tests


The bug, as the before-and-after code change required to fix it (changed the fourth and sixth lines in the code block)
- Before
`static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }`
- After
`static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }`

Why does this fix the issue?
The error was that in the line `arr[i] = newArray[arr.length - i - 1];`, elements from `newArray` are being assigned to `arr`. 
This means that `arr` is being modified instead of `newArray`. However, we want to do the opposite - we want to reverse the 
elements of `arr` and store them in `newArray`. In addition, we had initialized `newArray` with default values (0), and its 
elements are not modified in the loop. As a result, `arr` is also just being assigned zeros. After we correct the line to 
`newArray[i] = arr[arr.length - i - 1];` , the reversed elements of `arr` are correctly assigned to `newArray`. In addition, 
changing the return statement to `return newArray;` now returns the correct reversed array.



Part 2:
4 interesting options to use the `find` command:

Option 1: -mtime
This command helps find files by age/time of modification. 

Example 1: Within the past 3 days
Input:
find . -mtime -3
Output:
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

Example 2: Exactly 1 day ago
Input:
find . -mtime 1
Output:
./~$llabus 19B 2024.docx
./Syllabus 19B 2024.docx
./Week5_B4_Lecture.pdf
./Bild 4 - Individual Writing Assignment .odt
./Writing2 Instructions.pdf
./Bild 4 - Individual Writing Assignment .pdf
./Bild 4 - Individual Writing Assignment -2.pdf
./Lecture 7 Section B.pdf

Option 2: -name
Helps find files/directories with a specific name 
Example 1: Finding a file
Input: find ./Downloads -name "P3.3.docx"
Output: ./Downloads/P3.3.docx
Example 2: Finding a directory
Input: find ./Downloads -name "wavelet"
Output: ./Downloads/wavelet
Option 3: -type
Helps specify whether we want to find a file or a directory
Example 1: Finding a file
Input: find ./Downloads/week1Discussion-main-2 -type f
Output:
./Downloads/week1Discussion-main-2/DemoArrayImpl.java
./Downloads/week1Discussion-main-2/.DS_Store
./Downloads/week1Discussion-main-2/DemoArray.java
./Downloads/week1Discussion-main-2/libs/junit-4.12.jar
./Downloads/week1Discussion-main-2/libs/hamcrest-core-1.3.jar
./Downloads/week1Discussion-main-2/README.md
./Downloads/week1Discussion-main-2/DemoArrayImplTester.java 
Example 2: Finding a directory 
Input: find ./Downloads/week1Discussion-main-2 -type d
Output: 
./Downloads/week1Discussion-main-2
./Downloads/week1Discussion-main-2/libs

Option 4: -empty
Helps find empty files (can help declutter)
Example 1: Finding empty files
Input:
find ./Documents -empty         
Output:
./Documents/.localized
./Documents/GitHub/lab3/.git/objects/info
./Documents/GitHub/lab3/.git/refs/tags
./Documents/GitHub/lab3/.git/branches
./Documents/MATLAB

Example 2: When there's no empty files in the specified directory
Input: find ./Downloads/week1Discussion-main -empty
Output: No output (because no empty file found)

Used ChatGPT and entered the following:
“Find 4 interesting command-line options for the find command, and give a 
couple examples as well”
Kept regenerating for more options and chose what I thought were 
the most useful 
