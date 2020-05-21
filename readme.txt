
   =============== deployment pre-requisite =====================

  
  dev envireonment 
  
  Artifactory location
  
  each property of the ligacy parameter file.
	
  any dependancy with the artifactory
  
  any software (java )is required for the deployment which should be availabe for the deployment
  
  any framework (node js , python , npm ) is required for the application 
  
  step for the deployment

  installation guide to understand the requirement. 

  pos i will create and i will present it once approve we will rollout.
  

  
     
=========================  submodules

[ujam@jenkinMaster ~]$ cd allgitProj
 
 allgitProj]$ git clone https://github.com/sksumanta/newSubScm.git

 allgitProj]$ git clone https://github.com/sksumanta/uat.git 
 
 allgitProj]$ cd newSubScm/
    
    $ git  submodule init
    $ git submodule add https://github.com/sksumanta/uat.git uat
    $ git commit -m "the ust submodule is added"
	$ git push origin master:master
