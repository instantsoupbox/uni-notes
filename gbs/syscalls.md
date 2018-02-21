# Einfuehrung (1)
`int printf(const char *format, ...);`

`ssize_t read(int fildes, void *buf, size_t nbyte);`

`ssize_t write(int fildes, const void *buf, size_t nbyte);`

`void *malloc(size_t size);` 

`void free(void *ptr);`

# Einfuehrung (2)
`FILE *fopen(const char *path, const char *mode);`

`int fclose(FILE *stream);`

`char *fgets(char *s, int size, FILE *stream);`

`int fflush(FILE *stream);`

`int scanf(const char *format, ...);`

`int strcmp(const char *s1, const char *s2);`

`int strncmp(const char *s1, const char *s2, size_t n);`

`int system(const char *command);`

## Prozesse
`pid_t getpid(void);`

`pid_t getppid(void);`

`pid_t fork(void)`

`pid_t waitpid(pid_t pid, int *wstatus, int options);`

`pid_t wait(int *wstatus);`