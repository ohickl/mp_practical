General Tips
============

- **Activating an Environment**: Use `conda activate env_name` to activate the conda environment.
- **Starting a SLURM Interactive Session**: Use `si -c [cores] -t [minutes]` to start an interactive session with the specified resources.
- **Making a File Executable**: Use `chmod +x filename` to make a script executable.
- **Copying Data to `/dev/shm`**: This allows for faster read/write operations as `/dev/shm` is a shared memory filesystem.
- **Navigating the File System**: Use `cd directory_name` to change directories and `ls` to list the contents of the current directory.
- **Viewing File Contents**: Use `cat filename` to display the contents of a file, or `less filename` for a paginated view.
- **Counting Lines in a File**: Use `wc -l filename` to count the number of lines in a file.
- **Searching for Keywords**: Use `grep 'keyword' filename` to search for a specific keyword within a file.
- **Checking Disk Usage**: Use `du -sh directory_name` to check the size of a directory and `df -h` to check disk space usage.
- **Creating Directories**: Use `mkdir directory_name` to create a new directory.
- **Copying Files and Directories**: Use `cp source destination` to copy files or directories.
- **Moving or Renaming Files**: Use `mv source destination` to move or rename files or directories.
