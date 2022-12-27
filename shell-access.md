# Providing Shell Acess to Ops Team for logging into Application Stack 

## Option 1: Creating users through User Data 
The traditional way of getting this done is by delegating ssh access to all instances under the account to a dedicated user for the Ops team. This will require us to modify the metadata
of the instances and specify the below commands so that a user for the Ops Team is created for all instances at the time of startup. 

Below block for code needs to be added in userdata configuration for all instances

```
cloud_final_modules:
- [users-groups,always]
users:
  - name: username
    groups: [ wheel ]
    sudo:     
      - "ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart httpd"
      - "ALL=(ALL) NOPASSWD: /usr/bin/cp /home/username/webserver_configuration.conf /etc/httpd/conf.d/"
    shell: /bin/bash
    ssh-authorized-keys: 
    - ssh-rsa AB3nzExample
 ```
 
 
 ## Option 2: SSM 
 However, this puts us in a compromising position as we have to manually manage the users and keys that we create for them. Instead, we can make use of AWS SSM to provide just in time access 
 for any user using the SSM Client installed in the EC2 Instance. 
 
 Steps: 
 Install SSM Agent on your instance at the time of launch. 
 Assign the necessary role to the EC2 Instance so that just in time access can be granted. 
