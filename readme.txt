

==========================================================================================

		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
		
		https://medium.com/the-devops-ship/custom-jenkins-dockerfile-jenkins-docker-image-with-pre-installed-plugins-default-admin-user-d0107b582577
		
==========================================================================================



Check the unique content in a file by comparing two unsorted files
_-----------++++-----------
awk 'NR==FNR{a[$0];next}!($0 in a)' xyz abc ------ it will display the unique content of abc file

Check the common line between two unsorted files
------------------------
Use the same awk command without    !    sign 

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


How to encrypt and decrypt the string in unix
==================================================

$ echo Hi | openssl enc -aes-128-cbc -a -salt -pass pass:wtf
U2FsdGVkX18qAdhqop1SffsewHue6EOPNKv9dXc/0rI=

$ echo U2FsdGVkX18qAdhqop1SffsewHue6EOPNKv9dXc/0rI= | openssl enc -aes-128-cbc -a -d -salt -pass pass:wtf
Hi
$



Propose a directory structure
=================================================

foDeploy
├── inventories				# inventory hold different environment
│   │── fint
│   │   ├── group_vars			# Here in group_vars we assign variables to particular groups
│   │   │    ├── all.yaml
│   │   └── hosts.yaml			# In hosts.yaml we will keep the severs need to be connected for deployment process	
│   └── prit
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
├── preDeploy.yaml			# the user has to execute these individual  playbooks
├── deployTheBuild.yaml			#==========  sample code in preDeploy.yaml ==========
└── readMe.txt					- import_playbook: books/sourcecontrol.yaml
						- import_playbook: books/loadvars.yaml
						- import_playbook: books/downloadBuild.yaml
						- import_playbook: books/verifybuild.yaml
                                        
all.yaml
====================

---
#   - Comments begin with the '#' character
#   - Blank lines are ignored
#   - Top level entries are assumed to be groups, start with 'all' to have a full hierarchy
#   - Hosts must be specified in a group's   hosts:
#     and they must be a key (: terminated)
#   - groups can have children, hosts and vars keys
#   - Anything defined under a host is assumed to be a var
#   - You can enter hostnames or IP addresses
#   - A hostname/IP can be a member of multiple groups
#   - ansible_user should have ssh 

# Ungrouped hosts, put them in 'all' or 'ungrouped' group
all:
  children:
    adminserver:
      hosts:
        admin1.nyc_rx.com:
          ansible_connection: local
          ansible_user: phyRx
    javaservers:
      hosts:
        admin1.nyc_rx.com:
          ansible_connection: local
          ansible_user: phyRx
        angel-admin.nyc_rx.com:
          ansible_user: jboss
        penn.nyc_rx.com:
          ansible_user: jboss
        shadow-admin.nyc_rx.com:
          ansible_user: phyRx
        queens-admin.nyc_rx.com:
          ansible_user: phyRx
    appservers:
      hosts:
        forge-admin.nyc_rx.com:
          ansible_user: jboss
        banshee-admin.nyc_rx.com:
          ansible_user: jboss
        angel-admin.nyc_rx.com:
          ansible_user: jboss
        cable-admin.nyc_rx.com:
          ansible_user: jboss
        cannonball-admin.nyc_rx.com:
          ansible_user: jboss
        chamber-admin.nyc_rx.com:
          ansible_user: jboss
        colossus-admin.nyc_rx.com:
          ansible_user: jboss
        beast-admin.nyc_rx.com:
          ansible_user: jboss
        cyclops-admin.nyc_rx.com:
          ansible_user: jboss
        dazzler-admin.nyc_rx.com:
          ansible_user: jboss
        iceman-admin.nyc_rx.com:
          ansible_user: jboss
        bishop-admin.nyc_rx.com:
          ansible_user: jboss
        longshot-admin.nyc_rx.com:
          ansible_user: jboss
        jubilee-admin.nyc_rx.com:
          ansible_user: jboss
        gambit-admin.nyc_rx.com:
          ansible_user: jboss
        joseph-admin.nyc_rx.com:
          ansible_user: jboss
        lifeguard-admin.nyc_rx.com:
          ansible_user: jboss
        maggot-admin.nyc_rx.com:
          ansible_user: jboss
        emmafrost-admin.nyc_rx.com:
          ansible_user: jboss
        magneto-admin.nyc_rx.com:
          ansible_user: jboss
        penn.nyc_rx.com:
          ansible_user: jboss
        teller.nyc_rx.com:
          ansible_user: jboss
    batchdeploy:
      hosts:
        admin1.nyc_rx.com:
          ansible_connection: local
          ansible_user: phyRx
    webservers:
      hosts:
        abrams-admin.nyc_rx.com:
          ansible_user: phyRx
        bradley-admin.nyc_rx.com:
          ansible_user: phyRx
        chaffee-admin.nyc_rx.com:
          ansible_user: phyRx
        bronx-admin.nyc_rx.com:
          ansible_user: phyRx
    webdeploy:
      hosts:
        admin1.nyc_rx.com:
          ansible_connection: local
          ansible_user: phyRx
    dbdeploy:
      hosts:
        admin1.nyc_rx.com:
          ansible_connection: local
          ansible_user: phyRx
    rddeploy:
      hosts:
        queens-admin.nyc_rx.com:
          ansible_user: phyRx




============================== commit github using ansible ===============================




- name: get diff of original and new files
  command: diff {{ staging.scm }}/{{ erx_domain }}/{{ erx_domain }}.yaml {{ staging.dir }}/staged/{{ erx_domain }}.yaml
  failed_when: yamlDiff.rc > 1
  register: yamlDiff

- name: check commit and push if there were changes
  block:
    - name: print diff
      debug:
        msg: "{{ yamlDiff.stdout }}"

    - name: update local scm copy
      command: mv {{ staging.dir }}/staged/{{ erx_domain }}.yaml {{ staging.scm }}/{{ erx_domain }}/{{ erx_domain }}.yaml
      args:
        removes: "{{ staging.dir }}/staged/{{ erx_domain }}.yaml"

    - name: Commit any changes to git
      shell: git commit -m "Updated via deployment of build {{ build_num }}" {{ erx_domain }}/{{ erx_domain }}.yaml
      args:
        chdir: "{{ staging.scm }}"

    - name: Push commit to git
      shell: git push
      args:
        chdir: "{{ staging.scm }}"
  when: yamlDiff.rc == 1



===============================================================================================================================================================
Ansible execute each role in different host
=====================================================
---
- hosts: groupa
  roles:
    - oracle

- hosts: groupb
  roles:
    - apache

- hosts: local
  connection: local
  tasks:
    - { debug: { msg: "Tasks to run locally" } }
