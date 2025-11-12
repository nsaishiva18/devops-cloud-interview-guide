# Different types of Variables in Linux

In Linux, variables are used to store data that can be referenced and manipulated by scripts and commands. There are several types of variables in Linux, each serving different purposes. Here are the main types of variables:

1. **Environment Variables**:
   - These are global variables that are available to all processes and shells. They are used to configure the behavior of the system and applications.
   - Examples: `PATH`, `HOME`, `USER`, `SHELL`
   - You can view environment variables using the `printenv` or `env` command.

2. **Shell Variables**:
   - These are local to the shell session and are not inherited by child processes. They are used to store temporary data for the duration of the shell session.
   - Examples: `PS1` (prompt string), `IFS` (internal field separator)

3. **User-defined Variables**:
   - These are variables created by users to store custom data. They can be either shell variables or environment variables, depending on how they are defined.
   - Example:

     ```bash
     MY_VAR="Hello, World!"
     export MY_VAR  # Makes it an environment variable
     ```

4. **Special Variables**:
   - These are predefined variables that have special meanings in the shell.
   - Examples:
     - `$?`: Exit status of the last command
     - `$$`: Process ID of the current shell
     - `$#`: Number of positional parameters
     - `$0`: Name of the script or shell

```sh

ls -ltr
echo $?

```

## Kill vs Kill -9 in Linux

kill and kill -9 are commands used to terminate processes in Linux, but they operate differently:
1. **kill**:
   - The `kill` command sends a termination signal (SIGTERM) to a process, allowing it to gracefully shut down. This gives the process an opportunity to clean up resources, save data, and perform any necessary shutdown procedures.
   - Example: `kill <PID>` where `<PID>` is the Process ID of the target process.

2. **kill -9**:
   - The `kill -9` command sends a SIGKILL signal to a process, forcing it to terminate immediately without any cleanup. This is a more aggressive way to kill a process and should be used as a last resort.
   - Example: `kill -9 <PID>` where `<PID>` is the Process ID of the target process.