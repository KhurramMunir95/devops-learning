Today Learning

Commands:
**cd**, **ls**, **pwd**, **mkdir**, **touch

ls:
Displays the list of contents of the current directory

ls -l (Displays a long listing format, showing file permissions, owner, size, and modification date)**


**ls -a (Lists all files, including hidden files (those starting with a dot))**


**ls -h (Formats file sizes into a human-readable layout (e.g., K, M, G) rather than bytes.)**


**ls -lh displays a detailed list with human-readable file sizes. It combines the -l (long listing format) and -h (human-readable sizes like K, M, G instead of exact bytes) flags.**


**ls -t sorts the contents by modification time, showing the newest files and folders first.**



**ls -R (Displays directory contents recursively, listing subdirectories as well)

pwd: 
The pwd command stands for Print Working Directory (or Present Working Directory)**


**mkdir:
mkdir command stands for "make directory". It is a fundamental command-line utility used to create new directories (folders) in Unix, Linux, macOS, and Windows operating systems.

mkdir -p (Parents): Creates nested directory paths. If any parent directories in the path do not exist, it creates them automatically instead of throwing an error.
Example:** mkdir -p \~/devops-learning/week-01-linux (using \~ will create nested directory paths inside home directory, using / at the start creates directory inside the root directory and without / and \~ will create new directory inside the current directory)


**touch:
The touch command is a quick way to create a new, empty file or update the timestamp of an existing file in Linux and macOS terminals.**

**Example: touch day-01.md (it will create a new file in the current directory)

This command will write inside the day-01.md file:
echo "This is the first day" > day-01.md

cd:
The cd (change directory) command is a built-in shell utility used to navigate between folders in almost all operating systems, including Linux, macOS, and Windows**



**cd Documents	(Moves you forward into a specific folder (e.g., "Documents"))**

**cd ..	(Moves you backward up one level to the parent directory)**

**cd ../..	(Moves you backward up two levels)**

**cd or cd \~	(Takes you instantly to your Home directory (on Unix/Linux/macOS))**

**cd -	(Switches back to the previous directory you were just in)**

**cd /	(Jumps directly to the root directory of the system)**



