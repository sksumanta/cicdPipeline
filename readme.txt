
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
  

=================================================

ssh error ( sh key add in svn or tfs)

host key ( one type do manualy)
vault password encrypt and keep it in .ssh 

     
=========================  submodules

[ujam@jenkinMaster ~]$ cd allgitProj
 
 allgitProj]$ git clone https://github.com/sksumanta/newSubScm.git

 allgitProj]$ git clone https://github.com/sksumanta/uat.git 
 
 allgitProj]$ cd newSubScm/
    
    $ git  submodule init
    $ git submodule add https://github.com/sksumanta/uat.git uat
    $ git commit -m "the ust submodule is added"
	$ git push origin master:master



Access the artifacts build directory configuration on Jenkins using Groovy script

==================================================================================





import groovy.json.JsonSlurper

 

def baseVersion = "10.1.0.5"

 

def url = "https://tools.myproject.com/artifactory/api/storage/my-enterpriserx/artifacts/"

def query = "?list&deep=0&depth=1&listFolders=1&includeRootPath=0"

def api_key = "AKCp5cbchkwqMJdpKWD55cFNb8zCjDFNdcXw1nvp9ZAgiCQNtrxFLqXPC8JeHtnweiwHH7nqE"

 

def jsonString = (url+baseVersion+query).toURL().getText(requestProperties: ['X-JFrog-Art-Api': api_key])

 

JsonSlurper slurper = new JsonSlurper()

def parsedJson = slurper.parseText(jsonString)

 

def myList = parsedJson.files.uri.toString()

       .replace(" ","")

       .replace("/","")

       .replace("[","")

       .replace("]","")

       .split(",")

       .toList()

       .reverse()

 

return myList



Propose a directory structure
=================================================

FNT_PIT_Deploy
├── inventories
│   └── uat
│       ├── group_vars
│       │    ├── all.yaml
│       └── hosts.yaml
├── roles
│   ├── GitScm
│   │   └── tasks
│   │       ├── gitscm.yaml
│   │       └── main.yaml
│   ├── loadVars
│   │   └── tasks
│   │       └── main.yaml
│   ├── downloadBuild
│   │   └── tasks
│   │       ├── main.yaml
│   │       └── pullBuild.yaml
│   └── deployBuild
│       └── tasks
│           ├── disposeBuild.yaml
│           └── main.yaml
├── books
│   ├── source_control.yaml
│   ├── loadVars.yaml
│   ├── downloadBuild.yaml
│   └── deployBuild.yaml
│
├── preDeploy.yaml
├── deployTheBuild.yaml
└── readMe.txt

