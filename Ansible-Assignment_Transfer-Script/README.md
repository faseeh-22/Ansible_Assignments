
# Ansible Assignment:

Through ansible playbook transfer and execute a script  on any specific location. 

To complete the assignment, follow the below steps.



## Prerequisites:
1. Ansible should be installed on the control machine.
2. SSH access is set up between the control machine and the remote host.
3. The remote host is reachable and configured, to allow Ansible connections.



## Steps:

**1.Create the Script:**

  Save the following script content in a file named `test.sh` on the control machine. This script will be transferred and executed on the remote host.

   ```
   #!/bin/bash

   echo "Hello, World!"
 ```


**2. Create the Ansible Playbook:**

  Save the following playbook content in a file named `transfer-script.yml` on the control machine.

   ```
   ---
   - name: Transfer and execute a script
 	hosts: remote_host
 	remote_user: test_user
 	become: true
 	tasks:
   	- name: Transfer the script
     	copy:
       	src: test.sh
       	dest: /home/test_user
       	mode: '0777'
   	- name: Execute the script
     	command: sh /home/test_user/test.sh
     	register: script_output
   	- name: Print script output
     	debug:
       	var: script_output.stdout
```      	 


**3. Run the Ansible Playbook:**

   Open a terminal on the control machine and navigate to the directory where you saved the playbook and script.

   Run the following command to execute the playbook:
 
   `ansible-playbook transfer-script.yml`
 
   Ansible will connect to the remote host, transfer the script, execute it, and then print the output of the script execution.

   You should see the `"Hello, World!"` message printed as part of the script output.



## Explanation of Playbook:

**- name:** Descriptive name for the playbook.

**- hosts:** Specifies the target remote host(s) where the tasks will be executed.

**- remote_user:** The remote user used to connect to the remote host.

**- become:** This allows the tasks to run with elevated privileges (usually as a superuser like `root`).

**- tasks:** Contains a list of tasks that Ansible will execute sequentially.



**- Transfer the Script:**
```	
 -copy: Copies a file from the control machine to the remote host.
	
 -src: Source file path on the control machine.
	
 -dest: Destination path on the remote host.
	
 -mode: Sets the file permissions to allow script execution.
```


**- Execute the Script:**
```	
 -command: Executes a command on the remote host.
	
  The script is executed using the "sh" command.
```


**- Print Script Output:**
```	
 -debug: Displays debug information.
	
 -var: Specifies the variable whose value should be displayed (script_output.stdout).
```




