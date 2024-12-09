# Scheduling

- One time jobs -> `at`
- Recurring jobs -> `cron`

## at

The `at` command is used to schedule a one-time job to run at a specific time. The `at` command reads commands from standard input or a file and executes them at a specified time.

```bash
# run a command at a specific time
echo "echo hello" | at 10:00 PM

# run a command at a specific time
at 10:00 PM
echo "echo hello"
<EOT>
```

The `atq` command is used to display the list of jobs that are scheduled to run using the `at` command.

The `atrm` command is used to remove a job that is scheduled to run using the `at` command.

```bash
# remove a job
atrm <job_number>
```

### at.deny and at.allow

- `/etc/at.deny` -> list of users who are not allowed to use the `at` command
- `/etc/at.allow` -> list of users who are allowed to use the `at` command

If neither file exists every user is allowed to use the `at` command.

## cron

The `cron` daemon is used to schedule recurring jobs to run at specific times. The `cron` daemon reads the `crontab` files to determine the schedule of jobs.

The `crontab` command is used to create, edit, and display the `crontab` files.

```bash
# edit the crontab file
crontab -e

# display the crontab file
crontab -l
```

The `crontab` file is a text file that contains the schedule of jobs. Each line in the `crontab` file represents a job that is scheduled to run at a specific time.

```bash
# m h day_of_the_month month day_of_week command
# if a field is set to * it means every value is accepted so * * * * * means every minute
# Example: run a script every day at 10:00 PM
0 22 * * * /path/to/script.sh
```

It is also possible to use predefined values for the time fields.

- `@reboot`: run the command at startup
- `@yearly`, `@annually`: run the command once a year
- `@monthly`: run the command once a month
- `@weekly`: run the command once a week
- `@daily`, `@midnight`: run the command once a day
- `@hourly`: run the command once an hour

```bash
# Example: run a script every day at 10:00 PM
@daily /path/to/script.sh
```

### /etc/cron.\*

> `/etc/crontab` contains all the cron jobs for the system and is not meant to be edited directly. Includes all of the information of the directories below.

- `/etc/cron.d/`: directory that contains system-wide `cron` files
- `/etc/cron.daily/`: directory that contains daily `cron` files
- `/etc/cron.hourly/`: directory that contains hourly `cron` files
- `/etc/cron.monthly/`: directory that contains monthly `cron` files
- `/etc/cron.weekly/`: directory that contains weekly `cron` files

> REDHAT uses `anacron` for daily, weekly, and monthly jobs

> `anacron` is used to run jobs that are scheduled to run at specific intervals. The `anacron` daemon reads the `anacrontab` file to determine the schedule of jobs.

```bash
# display the anacrontab file
cat /etc/anacrontab
```

### cron.deny and cron.allow

- `/etc/cron.deny` -> list of users who are not allowed to use the `cron` daemon
- `/etc/cron.allow` -> list of users who are allowed to use the `cron` daemon

If neither file exists every user is allowed to use the `cron` daemon.
