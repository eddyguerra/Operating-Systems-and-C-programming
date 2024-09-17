# Exercise: Counting Non-White-Space Characters in a File

## Author: Eddy Guerra John
## Date: 09/14/2024

## Problem Description
The goal of this exercise is to write a C program that counts the number of non-white-space characters in an input text file. The program should take command-line arguments, including an option (`-f` or `-s`) to specify whether the output should be displayed on the screen or written to an output file.

### Features of the Program:
1. **Input File**: The program reads from an input text file specified as an argument.
2. **Output Options**:
    - `-f` flag: Write the output (count of non-white-space characters) to a specified output file.
    - `-s` flag: Display the output on the screen.
3. **Error Handling**: The program handles incorrect or insufficient arguments by displaying an appropriate error message.

### Command Format:
- **For Output to a File**:
  ```bash
  ./count_non_ws -f inputfile outputfile
  ```
- **For Output to Screen**:
  ```bash
  ./count_non_ws -s inputfile
  ```

### Error Handling:
- If arguments are missing or incorrect, the program will display an error message and exit.

---

## C Program Code:

```c
/*
 * Author: Eddy Guerra John
 * Date: 09/14/2024
 * 
 * Program Description:
 * This program counts the number of non-white-space characters in an input text file.
 * It accepts command-line arguments to specify whether the output should be printed 
 * to the screen or written to an output file. The -s flag indicates screen output, 
 * while the -f flag indicates output to a file.
 * 
 * Limitations:
 * The program assumes the input file is valid and that the user provides proper arguments.
 * It does not handle edge cases such as missing files or permission issues for file access.
 */

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>

void count_non_whitespace_chars(FILE *input_file, FILE *output_file) {
    int ch;
    int non_whitespace_count = 0;
    
    // Read each character and count non-whitespace characters
    while ((ch = fgetc(input_file)) != EOF) {
        if (!isspace(ch)) {
            non_whitespace_count++;
        }
    }

    // Output the result
    if (output_file != NULL) {
        fprintf(output_file, "Number of non-whitespace characters: %d\n", non_whitespace_count);
    } else {
        printf("Number of non-whitespace characters: %d\n", non_whitespace_count);
    }
}

int main(int argc, char *argv[]) {
    // Check for insufficient arguments
    if (argc < 3) {
        fprintf(stderr, "Error: Insufficient arguments provided.\n");
        fprintf(stderr, "Usage: command -f inputfile outputfile OR command -s inputfile\n");
        return 1;
    }

    // Parse the command-line options
    char *flag = argv[1];
    FILE *input_file;
    FILE *output_file = NULL;

    // Handle '-f' option: output to file
    if (strcmp(flag, "-f") == 0 && argc == 4) {
        input_file = fopen(argv[2], "r");
        output_file = fopen(argv[3], "w");
        
        if (input_file == NULL || output_file == NULL) {
            fprintf(stderr, "Error: Failed to open input/output file.\n");
            return 1;
        }

        count_non_whitespace_chars(input_file, output_file);
        fclose(input_file);
        fclose(output_file);

    // Handle '-s' option: output to screen
    } else if (strcmp(flag, "-s") == 0 && argc == 3) {
        input_file = fopen(argv[2], "r");
        
        if (input_file == NULL) {
            fprintf(stderr, "Error: Failed to open input file.\n");
            return 1;
        }

        count_non_whitespace_chars(input_file, NULL);
        fclose(input_file);

    } else {
        // Invalid arguments
        fprintf(stderr, "Error: Invalid arguments provided.\n");
        fprintf(stderr, "Usage: command -f inputfile outputfile OR command -s inputfile\n");
        return 1;
    }

    return 0;
}
```

---

## Steps to Compile and Run the Program:

1. **Create the C file**:
   ```bash
   vi count_non_ws.c
   ```
    - Paste the provided code into the file using `vi`.

2. **Compile the C Program**:
    - Use the following command to compile:
      ```bash
      gcc -o count_non_ws count_non_ws.c
      ```

3. **Create an Input Text File**:
    - Create a text file named `input.txt` for testing:
      ```bash
      vi input.txt
      ```
    - Add some content, for example:
      ```
      This is a test input file with some white-space characters.
      ```

4. **Run the Program**:
    - To display the output on the screen:
      ```bash
      ./count_non_ws -s input.txt
      ```
    - To write the output to a file:
      ```bash
      ./count_non_ws -f input.txt output.txt
      ```

5. **Check the Output**:
    - **For `-s` flag**: The result will appear directly on the screen.
    - **For `-f` flag**: The result will be written to `output.txt`. Use `cat` to view the file:
      ```bash
      cat output.txt
      ```

## Example of Command Execution:

- **Output to Screen** (`-s` flag):
   ```bash
   ./count_non_ws -s input.txt
   ```

- **Output to File** (`-f` flag):
   ```bash
   ./count_non_ws -f input.txt output.txt
   ```

---

## Error Handling:

The program will print an error message and exit in the following cases:
1. If the user does not provide enough arguments.
2. If there is a failure in opening the input or output file.
3. If the user provides incorrect or invalid options.

---

## Conclusion:
This program is a simple example of command-line argument processing in C, demonstrating file handling, counting non-white-space characters, and output redirection to either a file or standard output.