Open PowerShell with administrator rights

Copy the Sysmon full file path, for example:
   `C:\Users\xyz\Documents\Sysmon`

In PowerShell, run the command:
```
cd 'C:\Users\xyz\Documents\Sysmon'
```
   
Then press **Enter**

This command changes the directory to the specified file path. Next, you can copy your Sysmon configuration file to this directory. After pasting the configuration file, you should see the changes reflected in the folder

Now, run `Sysmon64.exe` by simply typing it and pressing **Enter**

Please note that this will run Sysmon without installation and will not make any changes to the computer at this stage. To install Sysmon with your configuration file (`sysmonconfig.xml`), use the following command in PowerShell:

```Sysmon64.exe -i sysmonconfig.xml
```
Then press **Enter**

Once you have reviewed the license agreement, click **Agree** to continue

Sysmon installation is now complete and running. the operation can be verify through the PowerShell interface, as well as by checking the Services on the computer and the Event Viewer.

Now, Sysmon [System Monitoring] is up and running, ready to detect any malicious activity on the computer.
