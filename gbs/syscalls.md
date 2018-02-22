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
* `pid_t getpid(void);`

* `pid_t getppid(void);`

* `pid_t fork(void)`

* `pid_t waitpid(pid_t pid, int *wstatus, int options);`
    * `pid` the child process the caller wants to wait for
    * points to a location where `waitpid` can store a status value
* `pid_t wait(int *wstatus);`

## Scheduling & Synchronisation

* `int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);`

* `int pthread_join(pthread_t thread, void **retval);`

* `int pthread_yield(void);`

* `void pthread_exit(void *retval);`

## Prozesskommunikation

* `int sigemptyset(sigset_t *set);`

* `int sigaction(int sig, const struct sigaction *restrict act, struct sigaction *restrict oact);`

* `int setitimer(int which, const struct itimerval *restrict value, struct itimerval *restrict ovalue);`

* `void *calloc(size_t nmemb, size_t size);`

* `void *realloc(void *ptr, size_t size);`

* `void *alloca(size_t size);`