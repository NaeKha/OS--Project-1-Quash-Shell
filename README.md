# OS--Project-1-Quash-Shell 
-- Project Partners: Karimah Mohammed , Madison Malalchi 

Introduction
This report outlines the design choices and code documentation for a simple shell implementation developed in C. The shell, which we call Quash, aims to simulate basic functionalities of Unix-like shells, including executing commands, handling input/output redirection, piping, and background processes. The following sections describe the design decisions made during the development process and the structure of the code that supports these features.

Design Choices
The design of Quash centers around the principles of simplicity, modularity, and efficiency. The key design decisions are summarized below:

Modular Approach:
The code is organized into distinct functions that handle different aspects of shell operation. The main program loop is responsible for receiving user input and delegating tasks to specific functions, such as command parsing, built-in command execution, or redirection handling. This modularity allows for easier maintenance and testing.

Built-in Commands:
To minimize the need for external executables, Quash supports a set of built-in commands. These include basic commands such as cd, pwd, echo, and exit. Built-in commands are processed within the shell itself rather than spawning new processes, making them more efficient.

Process Management:
External commands are executed by forking a new process using fork() and replacing the child process’s image with the desired command using execvp(). This allows the shell to handle both built-in and external commands seamlessly. The parent process waits for the child process to finish unless the command is executed in the background, in which case the parent continues accepting new input.

Redirection and Pipes:
The shell supports both input and output redirection (<, >) and piping (|). When handling redirection, the shell uses dup2() to reroute input and output streams to the appropriate files. Piping involves creating a pipe with pipe(), redirecting the output of one command to the pipe, and feeding the pipe’s output to the next command.

Signal Handling:
To ensure proper shell behavior, we implemented signal handling for SIGINT (Ctrl+C) and SIGALRM. These signals are caught and processed to either interrupt the current operation or enforce a timeout on foreground tasks. Signal handling helps ensure that the shell is responsive and resilient to user interruptions.

Background Process Execution:
To run a process in the background, the user appends an & to the command. The shell recognizes this by checking the command string and sets a flag indicating that the process should run asynchronously. This enables the shell to continue accepting commands while the background process executes.

Code Documentation
The code is extensively commented to ensure clarity and maintainability. Each function is described with a header comment outlining its purpose, input parameters, and return values. Inline comments are used to explain complex logic, particularly in areas like file descriptor manipulation, redirection handling, and piping.

Main Loop:
The main loop starts by printing the prompt and reading user input using fgets(). After parsing the input, it delegates command execution to other functions based on the type of command. If a built-in command is detected, it calls the corresponding built-in command function; otherwise, it proceeds to execute an external command.

Command Parsing:
The parsing function processes user input by breaking it into tokens, handling spaces, quotes, and environment variable expansions. It also identifies if the command contains special characters like pipes or redirection operators and prepares the input accordingly for further processing.

Redirection and Pipes:
Functions like execute_redirection() and execute_pipe() handle redirection and piping, respectively. These functions manage the file descriptors and ensure that the proper data flow occurs between the user, the shell, and the executed commands.

Signal Handling:
Signal handlers are implemented to intercept SIGINT and SIGALRM signals. The signal handler for SIGINT ensures that a user interrupt does not crash the shell, while the handler for SIGALRM kills any process that runs longer than the predefined timeout.

Conclusion
The design of Quash reflects a balance between simplicity and functionality. By focusing on core shell features such as built-in commands, process management, redirection, and background execution, we have created a flexible and efficient shell that is easy to maintain and extend. The modular structure, extensive code documentation, and adherence to Unix conventions ensure that Quash can serve as a learning tool for understanding the underlying operations of a shell. Future improvements could include the addition of job control, improved error handling, and more advanced signal processing.

References

Kerrisk, M. (2010). The Linux Programming Interface. No Starch Press.
Stevens, W. R. (2005). Advanced Programming in the UNIX Environment. Addison-Wesley.
