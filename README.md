## Cron for Kodi

This addon consists of a plugin and a service that will let you schedule various Kodi functions to be run on timers of your choosing. Functions to run can basically be anything from the list of built in Kodi functions (http://kodi.wiki/view/List_of_built-in_functions). Examples include: 

* Rebooting
* Restart Kodi
* Take a Screenshot
* Run another Addon or Script
* Play Media
* Refresh RSS
* Send a Notification
* Set Volume
* Update Music/Video Libraries

Additionally you can specify your timer to display a notification when they run. 


### Running the addon

You can run the addon directly to bring up a GUI. The __Add Job__ option will let you create a job, setting its name, command, and cron schedule. Clicking on an existing job will allow you to edit it's properties. Finally bringing up the context menu on a selected job will let you delete it. 

### Using as import in another addon

If you want to schedule something as part of your own addon you can import the CronManager class as an Kodi addon module. To do this first add the following to your addon.xml file: 

```xml 
<import addon="service.cronxbmc" version="Current.Version.Number" />
```

From within your addon import the required classes and modify jobs using the following example:


```python 
from cron import CronManager,CronJob

manager = CronManager()

#get jobs
jobs = manager.getJobs()

#delete a job
manager.deleteJob(job.id)

#add a job
job = CronJob()
job.name = "name"
job.command = "Shutdown"
job.expression = "0 0 * * *"
job.show_notification = "false"

manager.addJob(job)
```

Please be aware that adding or removing a job will change the job list (and change job ids) so please refresh your job list each time by using ```jobs = manager.getJobs()``` This will also pull in any new jobs that may have been added via other methods. 


### Manually Editing the cron.xml file

If you need to you can bypass the GUI and write the cron.xml file yourself, or via a script.  

The file should have the following layout:

```xml 
<cron>
 <job name="Job Name" command="Kodi_Command()" expression="* * * * *" show_notification="true/false" />
</cron>
```

Using Cron:

A Cron expression is made up for 5 parts (see below). Read up on cron(http://en.wikipedia.org/wiki/Cron) for more information on how to create these expressions.

    .--------------- minute (0 - 59)
    |   .------------ hour (0 - 23)
    |   |   .--------- day of month (1 - 31)
    |   |   |   .------ month (1 - 12) or Jan, Feb ... Dec
    |   |   |   |  .---- day of week (0 - 6) or Sun(0 or 7), Mon(1) ... Sat(6)
    V   V   V   V  V
    *   *   *   *  *
Example:
	0 */5 ** 1-5 - runs every five hours Monday - Friday
	0,15,30,45 0,15-18 * * * - runs every quarter hour during midnight hour and 3pm-6pm

