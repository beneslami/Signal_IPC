# Signal Programming
Signals in linux OS for Inter Process Communication

There are approximately 30 signals in linux environment. Here are some of the most well known signals which is used in system programming.

**SIGINT** -> Interrupt signal (Ctrl + C). Application gets terminated by receiving this signal.

**SIGUSR1** and **SIGUSR2** -> user defined signals. user defined process P1 sends a signal to another user defined process P2. These signals are left unused for users to define their signals.

**SIGKILL** -> This is a signal from the kernel which is sent to the process. The receiving process gets killed when it receives this signal from the kernel. Note that this signal cannot be caught by the process to perform its customized processing. So this signal is aggressive. This signal is created when the command "kill -9" is invoked.

**SIGABRT** -> This signal is raised by a specific system call called abort(). So whenever a process invokes abort() system call, it receives a signal from the OS which cannot be blocked, which means the process cannot perform its customized processing. As soon as the process receives this signal, the process is terminated.

**SIGTERM** -> it is the same as SIGKILL but the process can perform its own customized processing in response to the reception of this signal.

**SIGSEGV** -> Segmentation Fault. This signal is raised by the kernel to the process which is trying to write out of the scope of its own virtual memory address space.

**SIGCHILD** -> Whenever a child process gets terminated, this signal is sent to its parent. Uopn receiving this signal, the parent should execute wait() system call to read the child status.

###Three ways of generating Signals in linux:
1) Rising a signal from OS to a process (e.g Ctrl+C)

2) Sending a signal from process A to itself (using raise())

3) Sending signal from process A to process B (using kill())

**Signal Trapping**: A process can trap the signal received from OS and perform user defined function when signal is received.
> #include<signal.h>
signal(SIGINT, signal_handler);
static void signal_handler(int sig){
  printf("Ctrl+C pressed");
  exit(0);
}

**raise()**: A process can send a signal to itself using raise().
>int raise(int signo);

>static void ctrlC_signal_handler(int sig){
  printf("Ctrl-C pressed\n");
  printf("Bye Bye\n");
  exit(0);
}
static void abort_signal_handler(int sig){
  printf("process is aborted\n");
  printf("Bye Bye\n");
  exit(0);
}
int main(int argc, char \*argv[]){
  signal(SIGINT, ctrlC_signal_handler);
  signal(SIGABRT, abort_signal_handler);
  char ch;
  printf("Abort process (y/n) ?\n");
  scanf("%c", &ch);
  if(ch == 'y')
    abort();
  return 0;
}

Note that, it is mandatory to terminate the process by calling exit(0) from abort_signal_handler(). for SIGABRT signal must not return to caller. It returns, Kernel will kill the process instead. You can perform experiment by removing the exit(0) in above code, and you will notice that process is terminated after execution of abort_signal_handler(). Hence, Process either commit suicide or it will be killed by the OS. SIGABRT signal cannot be blocked(= ignore) by the process.

**Kill()**: A process can send a signal from its own user space to another process's user space by using kill() system call.
> int kill(int process_id, int signo);
