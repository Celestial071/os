## Describe and discuss the use cases of following

# Operating System Function Descriptions

## 1. fork()

The `fork()` system call is vital in operating systems for process creation. It duplicates the calling process, resulting in a new process called the child process, while the original process is referred to as the parent process. The `fork()` call is used to create a new process that runs concurrently with the parent process. After the child process is created, both the parent and child processes continue executing from the instruction following the `fork()` call.

## 2. exec()

The `exec()` family of functions replaces the current process image with a new one, running an executable file within the context of the current process and overwriting the previous executable. This command is often used to replace the running process with a new process, effectively terminating the current process and starting a new one.

## 3. getpid()

The `getpid()` function is simple and does not require any arguments. It returns the Process ID (PID) of the calling process. In a situation where a parent process creates a child process using `fork()`, `getpid()` can be used to retrieve the PID of the process, aiding in managing and coordinating between the parent and child processes.

## 4. wait()

The `wait()` function is employed by a parent process to wait for its child processes to terminate. When the parent process calls `wait()`, it suspends its execution until one of its child processes exits or a signal is caught. This function is essential in scenarios where a parent process needs to manage multiple child processes, ensuring proper synchronization by waiting for each child process to finish.

## 5. stat()

The `stat()` function provides detailed information about a file or directory, including size, permissions, ownership, and timestamps. It is widely used in system programming and file management. By calling `stat()`, information about a file is stored in a structure that includes various attributes, facilitating detailed file management operations.

## 6. opendir()

The `opendir()` function opens a directory stream, which can be used to read the directory's contents using functions like `readdir()` and to close the stream with `closedir()`. This function is typically used in applications that need to analyze or manage directory contents, such as file managers, backup tools, or custom scripts.

## 7. readdir()

The `readdir()` function reads entries from a directory stream that was opened with `opendir()`. It allows for iterating through directory contents and retrieving information about each file or subdirectory. This is useful for applications that need to list and process files in a directory.

## 8. close()

The `close()` function is used to close a file descriptor, an integer handle used by the operating system to reference an open file, socket, or other I/O resource. Closing a file descriptor releases the associated resources. It is important for ensuring that system resources are properly managed and not left open, which could lead to resource leaks.

---

# C Program for Producer-Consumer Problem

```c
#include <stdio.h>
#include <stdlib.h>

// Initialize a mutex variable to 1
int mutex = 1;

// Number of full slots initialized to 0
int full_slots = 0;

// Number of empty slots set to buffer size
int empty_slots = 10, item = 0;

// Function to produce an item and add it to the buffer
void producer() {
    --mutex;  // Lock the mutex
    ++full_slots;   // Increase the count of full slots
    --empty_slots;  // Decrease the count of empty slots
    item++;
    printf("\nProducer produces item %d", item);
    ++mutex;  // Unlock the mutex
}

// Function to consume an item and remove it from the buffer
void consumer() {
    --mutex;  // Lock the mutex
    --full_slots;   // Decrease the count of full slots
    ++empty_slots;  // Increase the count of empty slots
    printf("\nConsumer consumes item %d", item);
    item--;
    ++mutex;  // Unlock the mutex
}

int main() {
    int choice;
    while (1) {
        printf("\n1. Press 1 for Producer");
        printf("\n2. Press 2 for Consumer");
        printf("\n3. Press 3 for Exit");
        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                if (mutex == 1 && empty_slots != 0) {
                    producer();
                } else {
                    printf("Buffer is full!");
                }
                break;
            case 2:
                if (mutex == 1 && full_slots != 0) {
                    consumer();
                } else {
                    printf("Buffer is empty!");
                }
                break;
            case 3:
                exit(0);
                break;
            default:
                printf("Invalid choice! Please try again.");
                break;
        }
    }
    return 0;
}
