
java.lang.Boolean batchFinished = false
pipeline {
    agent { label 'master' }
    options {
        timestamps()
        //ansiColor('xterm')
        buildDiscarder(logRotator(daysToKeepStr: '35'))
    }
    environment {
        //ws = "${env.WORKSPACE}"
        JIRA_SITE = "jira"
        PATH = "/usr/local/enterprismyApp/ops-tools/cmPyEnv/bin:${JAVA_HOME}/bin:${ORACLE_HOME}/bin:${PATH}"
    }
	// parameters { }
	stages {
	    stage("CheckoutAnsible"){
			steps{
				script{
					// Lets clone the source code from git
					git 'https://github.com/sksumanta/ansibleDemoPoject.git';
				}
			}
		}
        stage('Initialize') 
		{
			steps {
                echo "Initialize"
                echo "Checking out SCM Files"
                //sh "cd ${env.WORKSPACE}/realProject"
                ansiblePlaybook installation: "ansible", colorized: true, forks: 15, sudoUser: null,
                                playbook: "${env.WORKSPACE}/realProject/books/source_control.yaml", inventory: "${env.WORKSPACE}/realProject/inventories/uat",
                                extras: "-e tier=uat -e mydomain=${source}",
                                tags: "checkout"'
 
			}
			
		}
	}
}
