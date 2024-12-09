# Jobs

## Jobs command

The `jobs` command is used to display the status of jobs that are running in the background. The output of the `jobs` command includes the job number, the status of the job, and the command that was used to start the job.

```bash
jobs

# output
[1]+  Running                 command &
[2]-  Running                 command2 &

jobs -p # display only the process IDs
```

## control-z

The `control-z` key combination is used to suspend a job that is running in the foreground. The job is paused and placed in the background. The job can be resumed by using the `fg` command.

## & ampersand

The `&` ampersand symbol is used to run a command in the background. The command is started in the background and the shell prompt is returned immediately. The job can be controlled using the `jobs` command.

```bash
command &
```

## fg command

The `fg` command is used to bring a job that is running in the background to the foreground. The job is resumed and the shell prompt is returned to the job. The job can be controlled using the `jobs` command.

```bash
fg 1 # bring job 1 to the foreground
```

## bg command

The `bg` command is used to resume a job that is running in the background. The job is resumed and the shell prompt is returned to the job. The job can be controlled using the `jobs` command.

```bash
bg 1 # resume job 1 in the background
```
