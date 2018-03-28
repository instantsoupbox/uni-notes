# POSIX (Portable Operating System Interface) Funktionen

## Einfuehrung (1)
* `int printf(const char *format, ...);`

* `ssize_t read(int fildes, void *buf, size_t nbyte);`

* `ssize_t write(int fildes, const void *buf, size_t nbyte);`

* `void *malloc(size_t size);` 

* `void free(void *ptr);`

## Einfuehrung (2)
* `FILE *fopen(const char *path, const char *mode);`

* `int fclose(FILE *stream);`

* `char *fgets(char *s, int size, FILE *stream);`

* `int fflush(FILE *stream);`

* `int scanf(const char *format, ...);`

* `int strcmp(const char *s1, const char *s2);`

* `int strncmp(const char *s1, const char *s2, size_t n);`

* `int system(const char *command);`

## Prozesse
* `pid_t getpid(void);`

* `pid_t getppid(void);`

* `pid_t fork(void)`

* `pid_t waitpid(pid_t pid, int *wstatus, int options);`
    * `pid` the child process the caller wants to wait for
    * points to a location where `waitpid` can store a status value
* `pid_t wait(int *wstatus);`

## Scheduling & Synchronisation

* `#include <pthread.h>` und `-lpthread` beim Kompilieren 

    * `int pthread_create (pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);`
        * `thread`: Pointer zum startenden Thread, für den bereits Platz allokiert ist.
        * `start_routine`: Funktionspointer auf die vom Thread auszuführende Funktion.
        * `arg` wird der auszuführenden Funktion mit übergeben.

    * `int pthread_join (pthread_t thread, void **retval);`

    * `int pthread_yield (void);`

    * `void pthread_exit (void *retval);`

* `#include <semaphore.h>`

    * `int sem_init (sem_t *sem, int pshared, unsigned int value);`

    * `int sem_wait (sem_t *sem);`
        * P-Operation

    * `int sem_post (sem_t *sem);`
        * V-Operation

## Prozesskommunikation

* `int pipe (int pipefd[2]);`

* `int socket (int domain, int type, int protocol);`

* `int connect (int sockfd, const struct sockaddr *addr, socklen_t addrlen);`

* `int bind (int sockfd, const struct sockaddr *addr, socklen_t addrlen);`

* `int listen (int sockfd, int backlog);`

* `int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);`

* `int close(int fd);`

* `int select (int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *utimeout);`

* `int epoll_create1(int flags);`

* `int epoll_ctl (int epfd, int op, int fd, struct epoll_event *event);`

* `int epoll_wait (int epfd, struct epoll_event *events, int maxevents, int timeout);`

* `int sigemptyset (sigset_t *set);`

* `int sigaction (int sig, const struct sigaction *restrict act, struct sigaction *restrict oact);`

* `int setitimer (int which, const struct itimerval *restrict value, struct itimerval *restrict ovalue);`

* `void *calloc (size_t nmemb, size_t size);`

* `void *realloc (void *ptr, size_t size);`

* `void *alloca(size_t size);`