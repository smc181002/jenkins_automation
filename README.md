# jenkins_automation

This project is done as a task of MLOps for learning DevOps automation for learning git, jenkins and docker.

## setup
for setting up the project first there are somethings to configure the linux:
* create two directories. Enter the commands `mkdir /root/testcode` and `mkdir /root/webcode`
* the code to be tested is saved in testcode folder and the code the client uses is stored in webcode
* though not necessary, we create a hook for pushing the code for pushing automatically using post-commit hook
* we create a job for extracting the code from the github form master branch using the git plugin and then create a token session for creating git hooks after post commit. we need to add a code using curl to communicate to jenkins as is a web app. 
the code for doing this is `curl --user '<admin>:<password> 'http://<IPAddress>/job/clone-files/build?token=TokenName'`
* similarly we also create a job for extracting the developer code which is to be delpoyed in the testing server. after creating a job  similar to the above one, we also need to add this curl command  `curl --user '<admin>:<password> 'http://<IPAddress>/job/clone-files/build?token=TokenName'` we can change the token name if we want
* then we make two jobs: one for hosting the code from master branch to a server which a client can access and other similar job to host it in testing container. Before this we need to create the containers at the start. so the following commands should be executed first:
`sudo docker run -itd -p 8081:80 -v /root/webcode:/usr/local/apache2/htdocs/ --name mastercontainer httpd` for client server and `sudo docker run -itd -p 8082:80 -v /root/testcode:/usr/local/apache2/htdocs/ --name testcontainer httpd` for testing server.
* then we create a final job for the merging the two branches. This job is built after QA team has finalised that there is no error
