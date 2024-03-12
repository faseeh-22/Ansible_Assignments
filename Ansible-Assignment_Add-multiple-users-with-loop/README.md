# Assignment:

Create an ansible playbook to add multiple user on target machine using loop directive.To complete this assignment follow the following steps:



## Prerequisites:

You should have SSH access to the target machine with appropriate privileges to add users (e.g., sudo access).
 


## Step 1: Create the Playbook File

1. Open a text editor or IDE of your choice.
2. Create a new file with the extension .yml (e.g., add-users.yml)
3. **Cmd:**
`vim add-users.yml`
4. and write the following contents in the file.

```
---
- name: Add multiple users to target machine
  hosts: remote_host
  become: true
  vars:
	users_to_add:
  	- name: Bob
    	password: dxdog
  	- name: John
    	password: testx
  	// Add more users here if needed
  tasks:
	- name: Create users
  	user:
    	name: "{{ item.name }}"
    	password: "{{ item.password | password_hash('sha512') }}"  # Passwords should be hashed
    	state: present
  	loop: "{{ users_to_add }}"
 ```

5. Save the playbook file by pressing “esc” key and typing “:wq”
 


## Step 2: Customize the Playbook

1. Modify the “remote_host” with the target machine's hostname or IP address in the playbook. Replace it with the actual hostname or IP address of your target machine.
2. In the `vars` section, define the list of users you want to add with their respective passwords.
3. In the “users_to_add” variable, you can add more users if needed. Just follow the same structure of name and password for each user you want to add.
4. It is essential to hash the passwords using Ansible's “password_hash” filter to ensure the security of the passwords. In the example above, I used the SHA-512 hashing algorithm, but you can choose other supported algorithms like “sha256”, “bcrypt”, etc.

**NOTE:** 
loop: "{{ users_to_add }}": This is the loop directive that iterates over the users_to_add list, and for each item in the list (each user), the "Create users" task is executed.
 
 

## Step 3: Set Up the Inventory File

1. Create an inventory file (e.g., invent.ini) and add the target machine's IP or hostname under the remote_host group.
2. **Cmd:**
`vim invent.ini`
3. Then define a group and write  ip address or hostname of the target machine.
```
[remote_host]
target_machine_ip_or_hostname
e.g  192.168.0.0.1
``` 
 
 
## Step 4: Run the Playbook

Open a terminal and navigate to the directory where you saved the playbook file (add_users.yml) and the inventory file (invent.ini).
 
Now, execute the playbook using the ansible-playbook command:
 
`ansible-playbook -b  -i invent.ini add_users.yml -kK`
 
 
 
## Step 5: Provide SSH Password 

 Ansible will prompt you to enter the SSH password , depending on your configuration.
 
## Step 6: Ansible Execution

 Ansible will execute the playbook, and for each user in the “users_to_add” list, it will create the user with the specified name and securely hashed password.
 


## Step 7: Verify Users on Target Machine

 After the playbook execution is completed, log in to the target machine and verify that the specified users have been successfully added.
 



