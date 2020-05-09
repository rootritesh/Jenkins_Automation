# Jenkins_Automation
Jenkins_Automation_To_test_deploy_and_run

#Prerequisite
Step -1)
#install Git
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

$Add git repo and configure using dev1 branch.
$Add Poll SCM using * * * * *
$Add this code in the executable shell:

sudo cp -v -r -f * /root/new 
if sudo docker ps -a | grep webodev1
then
echo running
else
sudo docker run -dit --net=host -p 8082:80 -v /root/new:/usr/local/apache2/htdocs/ --name webosdev1 httpd
fi
sudo docker inspect webosdev1 | grep IP

$Save it.

#job2:

#Create a freestyle project name the item job2
configure using following steps:

$Add git repo and configure using master branch.
$Add Poll SCM using * * * * *
$Add this code in the executable shell:

sudo cp -v -r -f * /root/newmaster
if sudo docker ps -a | grep webosmaster
then
echo "already running"
else
sudo docker run -dit --net=host -p 8082:80 -v /root/newmaster:/usr/local/apache2/htdocs/ --name webosmaster httpd
fi
sudo docker inspect webosmaster

$Save it.

#job3:

#Create a freestyle project name the item job3
configure using following steps:

$Add git repo and configure using master,dev1 branch.
$Add build trigger under the option (Build after other projects are built) add job1,job2 and tick option Trigger only if build is stable

$Add this code in the executable shell:

git merge origin/dev1
sudo cp -v -r -f * /root/mainenv
if sudo docker ps -a | grep webosenv
then
echo "already running"
else
sudo docker run -dit --net=host -p 8082:80 -v /root/mainenv:/usr/local/apache2/htdocs/ --name webosenv httpd
fi
sudo docker inspect webosenv

$Save it.

  

