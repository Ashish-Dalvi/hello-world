# Deploy on Docker host server using Jenkins 
# *Jenkins Job name:* `Deploy_on_Docker_Host`

### Pre-requisites

1. Jenkins server 
1. Docker-host Server 

### Integration between Docker-host and Jenkins

Install "publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Enable connection between Docker-host and Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers` 

	- SSH Servers:
                - Name: `docker-host`
		- Hostname:`<ServerIP>`
		- username: `dockeradmin`

       -  `Advanced` > chose `Use password authentication, or use a different key`
		 - password: `*******`

### Steps to create "Deploy_on_Docker_Host" Jenkin job
 #### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Docker_Host`
     
   - *Source Code Management:*
      - Repository: `https://github.com/Ashish-Dalvi/Hello-World.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `docker-host`
       - `Transfers` >  `Transfer set`
            - Source files: `webapp/target/*.war`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `//home//ansadmin` or `.`
         - Exec command: `cd /home/dockeradmin; docker build -t devops-image .; docker run -d --name=devops-cont -p 8080:8080 devops-image;`


Save and run the job now.

### Link to access Hello World 
http://18.181.174.173:8080/webapp/
