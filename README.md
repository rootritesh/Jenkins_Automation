# Jenkins_Automation
Jenkins_Automation_To_test_deploy_and_run

#Prerequisite
Step -1)
#install Git and make branch using command:git branch dev1
#install jenkins
#install docker

Step-2)
#Start the services
#systemctl start jenkins
#systemctl start docker

Step-3)
Make the 3 jobs

#job1:

#Create a freestyle project name the item job1
configure using following steps:

$Add git repo and configure using dev1 branch.<br>
$Add Poll SCM using * * * * *<br>
$Add this code in the executable shell:<br>

sudo cp -v -r -f * /root/new <br>
if sudo docker ps -a | grep webodev1<br>
then<br>
echo running<br>
else<br>
sudo docker run -dit --net=host -p 8082:80 -v /root/new:/usr/local/apache2/htdocs/ --name webosdev1 httpd<br>
fi<br>
sudo docker inspect webosdev1 | grep IP<br>

$Save it.

#job2:

#Create a freestyle project name the item job2
configure using following steps:

Add git repo and configure using master branch.<br>
Add Poll SCM using * * * * *<br>
Add this code in the executable shell:<br>

sudo cp -v -r -f * /root/newmaster
if sudo docker ps -a | grep webosmaster<br>
then<br>
echo "already running"<br>
else<br>
sudo docker run -dit --net=host -p 8082:80 -v /root/newmaster:/usr/local/apache2/htdocs/ --name webosmaster httpd<br>
fi<br>
sudo docker inspect webosmaster<br>

$Save it.

#job3:

#Create a freestyle project name the item job3<br>
configure using following steps:

$Add git repo and configure using master,dev1 branch.<br>
$Add build trigger under the option (Build after other projects are built) add job1,job2 and tick option Trigger only if build is stable<br>

$Add this code in the executable shell:<br>

git merge origin/dev1<br>
sudo cp -v -r -f * /root/mainenv<br>
if sudo docker ps -a | grep webosenv<br>
then<br>
echo "already running"<br>
else<br>
sudo docker run -dit --net=host -p 8082:80 -v /root/mainenv:/usr/local/apache2/htdocs/ --name webosenv httpd<br>
fi<br>
sudo docker inspect webosenv<br>

$Save it.<br>

  

