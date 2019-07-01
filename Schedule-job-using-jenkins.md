We use jenkins as replacement of [crontab](https://help.ubuntu.com/community/CronHowto) to schedule regular jobs for our project.

To add more jobs, do these steps:

## 1. Ask for jenkins permission from sys admin

## 2. Set up and configure a job, feel free to use that easy UI

For more info, we can check [this](http://www.davedevelopment.co.uk/2015/06/04/scheduled-tasks-with-jenkins.html)

## 3. Schedule it

We schedule job by config a crontab. For example, this config will run the job daily at any time from 2:00-3:00 am, where scheduler find that the computer has free resource.
```
H 2 * * *
```