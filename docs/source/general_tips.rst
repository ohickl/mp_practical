General Tips
============

- **Activating an Environment**: Use `conda activate env_name` to activate the conda environment.
- **Starting a SLURM Interactive Session**: Use `si -c [cores] -t [minutes]` to start an interactive session with the specified resources.
- **Making a File Executable**: Use `chmod +x filename` to make a script executable.
- **Copying Data to `/dev/shm`**: This allows for faster read/write operations as `/dev/shm` is a shared memory filesystem.
