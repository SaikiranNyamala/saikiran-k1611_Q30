# saikiran-k1611_Q30

30.	Design a program using concepts of inter-process communication ordinary pipes in which one process sends a string message to a second process, and the second process reverses the case of each character in the message and sends it back to the first process. For example, if the first process sends the message Hi There, the second process will return hI tHERE. This will require using two pipes, one for sending the original message from the first to the second process and the other for sending the modified message from the second to the first process. You can write this program using either UNIX or Windows pipes.  


CODE:

#include <sys/types.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
 
#define BUFFER_SIZE 25
#define READ_END 0
#define WRITE_END 1
#define READ_END2 0
#define WRITE_END2 1
 
int main(void)
{
  int ch = 0;
  int ch2 = 0;
  char write_msg[BUFFER_SIZE] = "Greetings";
  char write_msg2[BUFFER_SIZE];
  char read_msg[BUFFER_SIZE];
  int fd[2];
  int fd2[2];
  pid_t pid;
  if (pipe(fd) == -1) {
    fprintf(stderr, "Pipe failed");
    return 1;
  }
  if (pipe(fd2) == -1) {
    fprintf(stderr, "Pipe failed");
    return 1;
  }
  pid = fork();
 
  if (pid < 0) {
    fprintf(stderr, "Fork failed");
    return 1;
  }
 
  if (pid>0){
    close(fd[READ_END]);
    close(fd2[WRITE_END2]);
     
    while (write_msg[ch] != 0)
    {
      write(fd[WRITE_END], &write_msg[ch], strlen(write_msg)+1);
      ch++;
    }
    close(fd[WRITE_END]);
    while (read_msg[ch] != 0)
    {
      read(fd2[READ_END2], &read_msg[ch2], 1);
      printf("%c", read_msg[ch2]);
      if (ch2%2 == 0) {
    ch2++;
      }
    }
  }
 
  else {
    close(fd[WRITE_END]);
    close(fd2[READ_END2]);
    ch = 0;
    ch2 =0;
    while (read_msg[ch] != 0)
    {
      read(fd[READ_END], &read_msg[ch], 1);
      write_msg2[ch] = toupper(&read_msg[ch]);
      printf("%c", read_msg[ch]);
      if (ch%2 == 0) {
    ch++;
      }
    }
    while (write_msg2[ch2] != 0)
    {
      write(fd2[WRITE_END2], &write_msg2[ch2], strlen(write_msg2)+1);
      ch2++;
    }
 
    close(fd2[READ_END]);
  }
  return 0;
}
