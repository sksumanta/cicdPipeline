
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

focusDeploy
├── inventories				# inventory hold different environment
│   │── fnt
│   │   ├── group_vars			# Here in group_vars we assign variables to particular groups
│   │   │    ├── all.yaml
│   │   └── hosts.yaml			# In hosts.yaml we will keep the severs need to be connected for deployment process	
│   └── pit
│       ├── group_vars
│       │    ├── all.yaml
│       └── hosts.yaml
├── roles				# This hierarchy represents a "roles"
│   ├── pullParameter			# This contain the code to pull environment parameter from Github
│   │   └── tasks
│   │       ├── pullEnvParameter.yaml
│   │       └── main.yaml
│   ├── loadVars			# The loadVars will load the parameters which pulled from Github
│   │   └── tasks			#========  sample code in loadVars/tasks/main.yaml ==========
│   │       └── main.yaml			- name: Load the {{ tier }} tier deployment variables
│   │       					  include_vars:
│   │       					    dir: '{{ staging.scm }}/{{ mydomain }}'
│   │       					    files_matching: "{{ mydomain | default('skipping') }}.yaml"
│   ├── downloadBuild			# Here we download Build
│   │   └── tasks
│   │       ├── main.yaml
│   │       ├── pullBuild.yaml
│   │       └── verifybuild.yaml
│   └── deployBuild			# we will keep business logic to deploy the build
│       └── tasks
│           ├── deployingBuild.yaml
│           ├── uploadParameterFile.yaml
│           └── main.yaml				
├── books				# All the playbooks will be executed from books 
│   ├── sourceControl.yaml
│   ├── loadVars.yaml
│   ├── downloadBuild.yaml
│   ├── verifybuild.yaml
│   └── deployBuild.yaml
│
├── sourceControl.yaml
├── preDeploy.yaml			# the user has to execute these indivisual playbooks
├── deployTheBuild.yaml			#==========  sample code in preDeploy.yaml ==========
└── readMe.txt					- import_playbook: books/sourcecontrol.yaml
						- import_playbook: books/loadvars.yaml
						- import_playbook: books/downloadBuild.yaml
						- import_playbook: books/verifybuild.yaml
                                        

