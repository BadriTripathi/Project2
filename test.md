---
- hosts: webservers
  tasks:
     - name: Run the equivalent of "apt-get update" as a separate step
       apt:
        update_cache: yes

     - name: Install NodeJS package
       apt: name={{ item }} state=latest 
       loop:
         - nodejs
         - unzip
         - npm
     
     - name: Copy NodeJS code on Application servers
       get_url: url=https://github.com/anujdevopslearn/SonarQubeNodeJS/archive/refs/heads/master.zip dest=/tmp/artifact.zip 


     - name: Extract Code
       unarchive: src=/tmp/artifact.zip dest=/opt/ remote_src=True


     - name: Run Node Install
       shell: |
        npm install 
        npm install forever -g
        forever start app/server.js
        forever list
       args:
         chdir:  /opt/SonarQubeNodeJS-master 
