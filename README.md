# Signal_IPC
Signals in linux OS for Inter Process Communication

There are approximately 30 signals in linux environment. Here are some of the most well known signals which is used in system programming.

**SIGINT** -> Interrupt signal (Ctrl + C). Application gets terminated by receiving this signal.

**SIGUSR1** and **SIGUSR2** -> user defined signals. user defined process P1 sends a signal to another user defined process P2. These signals are left unused for users to define their signals.

**SIGKILL** -> This is a signal from the kernel which is sent to the process. The receiving process gets killed when it receives this signal from the kernel. Note that this signal cannot be caught by the process to perform its customized processing. So this signal is aggressive. This signal is created when the command "kill -9" is invoked.

**SIGABRT** -> This signal is raised by a specific system call called abort(). So whenever a process invokes abort() system call, it receives a signal from the OS which cannot be blocked, which means the process cannot perform its customized processing. As soon as the process receives this signal, the process is terminated.

**SIGTERM** -> it is the same as SIGKILL but the process can perform its own customized processing in response to the reception of this signal.

**SIGSEGV** -> Segmentation Fault. This signal is raised by the kernel to the process which is trying to write out of the scope of its own virtual memory address space.

**SIGCHILD** -> Whenever a child process gets terminated, this signal is sent to its parent. Uopn receiving this signal, the parent should execute wait() system call to read the child status.
