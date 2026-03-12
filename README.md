*This project has been created as part of the 42 curriculum by edmvarda.*

## Description

get_next_line is a 42 school project that challenges you to implement a function which reads a single line at a time from a file descriptor. Each successive call to get_next_line returns the next line from the file, including the terminating newline character (if present). The function must work correctly with any BUFFER_SIZE value defined at compile time, and must handle files, standard input, and edge cases such as empty files or files without a trailing newline.
Goal: Implement the following function:
cchar *get_next_line(int fd);
It returns the next line read from fd, or NULL on EOF or error.

## Algorithm

The implementation is built around a static stash + read-and-join approach:

Static stash — A static char *stash persists between calls to get_next_line. It accumulates any leftover data from the previous read that came after the last newline.
read_and_join — On each call, this function reads chunks of BUFFER_SIZE bytes from the file descriptor and appends them to the stash using ft_strjoin, until a newline character is found in the stash or EOF is reached.
extract_line — Once the stash contains a newline (or EOF is reached with remaining data), this function allocates and returns a new string containing exactly one line, including the \n if present.
clean_stash — After extraction, the stash is updated to contain only the data that came after the returned line's newline, ready for the next call.

Why this algorithm?
This design is memory-efficient and conceptually clean. The stash acts as a buffer between calls, meaning we never read more from the file than necessary, and we never discard data. The static variable ensures state is preserved across calls without requiring the caller to manage any context.

## Instructions

Compilation
bashcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c -o gnl_test
Usage Example
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int     fd;
    char    *line;

    fd = open("file.txt", O_RDONLY);
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return (0);
}
Files
FileDescriptionget_next_line.hHeader — prototypes and BUFFER_SIZE definitionget_next_line.cCore logic — get_next_line, read_and_join, join_read, extract_line, clean_stashget_next_line_utils.cHelper functions — ft_strlen, ft_strjoin, ft_strchr

## Resources

Linux read man page
Static variables in C — cppreference
Valgrind documentation
AI Usage 

AI was not used to write the initial implementation — it was used as a review and debugging tool after the code was written.