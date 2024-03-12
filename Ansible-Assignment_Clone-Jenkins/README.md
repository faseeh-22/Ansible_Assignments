

# Assignment:
(1) push playbook on github.   (2) push ansible config file and host file also.	(3) Create a jenkin pipeline, through jenkins clone all the things from github and run build pipeline.

To complete the assignment, follow the below steps.


     	 
## Step 1. Pushing Playbook to GitHub:

 
**1.Create a GitHub Repository:**
    
-Log in to your GitHub account.

-Click on the `+` icon in the top right corner and select 'New repository'.

-Choose a repository name (e.g 'Playbook2').

-Select repository options as needed.

-Click 'Create repository'.


**2.Push Playbook and other files to GitHub:**
    
-Open a terminal window on your local machine.

-Navigate to the directory containing your Ansible playbook file (nginx.yml), Ansible configuration file (ansible.cfg.new) and its host file (inventory.ini), and Jenkinsfile.

-Initialize a Git repository: `git init`

-Add your all files: `git add .`

-Commit your changes: `git commit -m "Initial commit"`

-Link the local repository to the GitHub repository: `git remote add origin https://github.com/faseeh-22/Playbook2.git`

-Push all the files to GitHub: `git push -u origin master`



## Step 2. Creating a Jenkins Pipeline:

 
**1.Login to Jenkins:**
    
-Ensure Jenkins is installed and running. Access it via a web browser at `http://localhost:8080`.

-Then login to Jenkins, by providing your credentials (username and password).

	
**2.Create a New Pipeline:**
    
-From the Jenkins dashboard, click on 'New Item'.

-Enter a name for your pipeline (e.g 'Ansible-Nginx-Deployment').

-Select 'Pipeline' and click 'OK'.

	
 **3.Configure Pipeline Settings:**
    
-Under the 'Pipeline' section, choose 'Pipeline script from SCM'.

-Select 'Git' for SCM.

-Enter your GitHub repository URL in the 'Repository URL' field.

-Choose the appropriate credentials for accessing the repository.

-Set the 'Script Path' to "Jenkinsfile".

	
 **4.Save and Build:**
    
-Click 'Save' to save the pipeline configuration.

-Click 'Build Now' to trigger the pipeline.

	

## Pipeline Execution:
    
The pipeline will start executing stages defined in the Jenkinsfile.
 
   	 

**NOTE:** Following are the contents of the user defined ansible hosts file "inventory.ini".
```
[all]
127.0.0.1 ansible_become_password=f********2
127.0.0.1 ansible_user=faseeh

[remote_host]
127.0.1.1 test_user_become_password=f********2
127.0.1.1 test_user=test_user
```

	 
**NOTE:** The ansible configuration file "ansible.cfg.new" is empty and it is a user defined file. But we can run ansible playbooks with empty ansible configuration files.


   	 
**NOTE:** Following are the contents of the ansible playbook "nginx.yml".
``` 
- hosts: localhost
  connection: local
  become: true
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: latest
    - name: Start the service
      service:
        name: nginx
        state: started
   ```	 


**NOTE:** Following are the contents of Jenkinsfile.
```
pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                git credentialsId: 'git', url: 'https://github.com/faseeh-22/Playbook2.git'
            }
        }

        stage('Run playbook') {
            steps {
                sh 'ansible-playbook -b -i invent.ini nginx.yml'
            }
        }
    }
}
``` 
 
  	 
## Jenkinsfile Explanation:

	
**Agent Section:** The pipeline is configured to run on any available agent.

	
**Stages:**

**-Clone Repository:** The pipeline's first stage is to clone the GitHub repository using the Git plugin.

**-Run Playbook:** The second stage uses a shell step to run the Ansible playbook using the ansible-playbook command. The "-b" flag indicates privilege escalation (become), and the "-i" flag specifies the inventory file (inventory.ini). The playbook itself is specified as "nginx.yml".
   	 
   	 
    	


