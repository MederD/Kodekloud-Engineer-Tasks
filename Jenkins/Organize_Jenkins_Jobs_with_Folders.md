xFusionCorp Industries' DevOps team aims to streamline the management of Jenkins jobs by organizing them into distinct folders based on their purpose. Complete the task following the provided requirements:  
1.Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.  
2. Create a new folder named Apache within the Jenkins UI.  
3. Move the existing jobs httpd-php and services under the newly created Apache folder.  
Note:  
1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page.  
2. Be aware that Jenkins UI may experience temporary unresponsiveness during the service restart. Refresh the UI page if needed.  
3. Capture screenshots of your work for documentation and review purposes. Alternatively, utilize screen recording software like loom.com for detailed documentation and sharing.

Solution:  
```
Manage Jenkins > Manage Plugins.
Under the Installed tab, search for Folders plugin.
If not present, go to the Available tab, search for Folders, and install it.

2. Create a New Folder Named "Apache"
From the Jenkins dashboard, click New Item.
Enter the name: Apache.
Select Folder as the item type (if you don't see this, ensure the Folders plugin is installed).

3. Move Existing Jobs (httpd-php and services) to "Apache" Folder
On the Jenkins dashboard, locate the job (httpd-php).
Click the job name, click Move.
In the dropdown, select the Apache folder as the destination.
```

[reference](https://plugins.jenkins.io/cloudbees-folder/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Task)
