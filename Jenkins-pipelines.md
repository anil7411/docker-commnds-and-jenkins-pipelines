# Syntax
```sh
pipeline {
 agent any
   stages {
     stage("stage1") {
	  steps {
	    echo "This is Stage1"
        }
	 }
	 stage("stage2") {
	  steps {
	    echo "This is Stage2"
        }
	 }
	 stage("stage3") {
	  steps {
	    echo "This is Stage3"
        }
	 }
	 ;;;;
	 ;;;;
	 ;;;;
	}
}	
```

agent section:-
--------------
pipeline {
 agent any
 ;;
 ;;
 ;;
---------------------------------------------------

pipeline {
 agent {label "labelname") 
 ;;
;;
;;
} 
---------------------------------------------------
Stage-level Agent Section:-
--------------------------
pileline {
 agent none
 stages {
  stage("git clone") {
   agent {label "slave1"}
   steps {
     logic
   }
  }
  stage("package") {
   agent {label "labelName2"}
   steps {
     logic
    }
  }
 }
}
====================================================================================================================================
post section:-
-------------
The post section defines one or more additional steps that are run upon the completion of a Pipeline’s or stage’s run

post section allowed in the top-level pipeline block and each stage block.

pileline {
  agent none
  stages {
   stage("stage1") {
    steps {
	  echo "This is stage1"
	}
   }
	stage("stage2") {
    steps {
	  echo "This is stage2"
	}
   }
  }
  post {
   always {
     echo "this block will be executed regardless of the completion status of the Pipeline’s or stage’s run
    }
   success {
    echo "this block will be executed when the current Pipeline’s or stage’s run has a "success" status, 
	      typically denoted by blue or green in the web UI."
   }
   failure {
    echo "this block will be executed the current Pipeline’s or stage’s run has a "failed" status, typically denoted by red in the web UI"
   }
   aborted {
    echo "this block will be executed when the current Pipeline’s or stage’s run has an "aborted" status, usually due to the Pipeline being manually aborted. 
	      this is typically denoted by gray in the web UI"
   }
   regression {
    echo "Only run the steps in post if the current Pipeline’s or stage’s run’s status is failure, unstable, or aborted and the previous run was successful."
   }
   cleanup {
     echo "this block will be executed regardless of the Pipeline or stage’s status.
   }
   change {
      echo "This block will be executed when the current Pipeline’s or stage’s run has a different completion status from its previous run
   }
   fixed {
    echo "This block will be executed when the current Pipeline’s or stage’s run is successful and the previous run failed or was unstable"
   }
  }
}
======================================================================================================================================================================
options directive:-
-----------------
The options directive allows configuring Pipeline-specific options from within the Pipeline itself.
Allowed: Only once, inside the pipeline block.

Available Options:-

buildDiscarder:-
--------------
Persist artifacts and console output for the specific number of recent Pipeline runs. 
For example: options { buildDiscarder(logRotator(numToKeepStr: '1')) }

checkoutToSubdirectory:-
-----------------------
Allows you to override the location that the automatic SCM checkout will use. 
Using checkoutToSubdirectory("foo"), your Pipeline will checkout your repository to "$WORKSPACE/foo", rather than the default of "$WORKSPACE".

disableConcurrentBuilds:-
-----------------------
Disallow concurrent executions of the Pipeline. Can be useful for preventing simultaneous accesses to shared resources, etc. 
For example: options { disableConcurrentBuilds() }

retry:-
-----
On failure, retry the entire Pipeline the specified number of times. 
For example: options { retry(3) }

timeout:-
-------
Set a timeout period for the Pipeline run, after which Jenkins should abort the Pipeline. For example: options { timeout(time: 1, unit: 'HOURS') }

examples:-

pipeline {
    agent any
    options {
        timeout(time: 1, unit: 'HOURS') 
    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}

ex2:-
-----
pipeline {
    agent any
    stages {
        stage('Example') {
            options {
                retry(3) 
            }
            steps {
                echo 'Hello World'
            }
        }
    }
}

======================================================================================================================================================================

parameters directives:-
----------------------
The parameters directive provides a list of parameters that a user should provide when triggering the Pipeline

Available Parameters:-
--------------------
string:-
A parameter of a string type, for example: parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }

text:-
A text parameter, which can contain multiple lines, for example: parameters { text(name: 'DEPLOY_TEXT', defaultValue: 'One\nTwo\nThree\n', description: '') }

booleanParam:-
A boolean parameter, for example: parameters { booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: '') }

choice:-
A choice parameter, for example: parameters { choice(name: 'CHOICES', choices: ['one', 'two', 'three'], description: '') }

password:-
A password parameter, for example: parameters { password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'A secret password') }

examples:-
----------

pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
    }
}
========================================================================================================================================================
triggers directive:-
-------------------
The triggers directive defines the automated ways in which the Pipeline should be re-triggered

The triggers currently available are cron, pollSCM and upstream

cron:-
-----
Accepts a cron-style string to define a regular interval at which the Pipeline should be re-triggered, for example: triggers { cron('H */4 * * 1-5') }

pollSCM:-
-------
Accepts a cron-style string to define a regular interval at which Jenkins should check for new source changes. If new changes exist, the Pipeline will be re-triggered. 
For example: triggers { pollSCM('H */4 * * 1-5') }

upstream:-
--------
Accepts a comma-separated string of jobs and a threshold. When any job in the string finishes with the minimum threshold, the Pipeline will be re-triggered. 
For example: triggers { upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS) }

example:-
-------
pipeline {
    agent any
    triggers {
      
	  //cron('0 9-18 * * 1-5') //This job will be triggered  9am-7pm at 0 mins from mon-friday
      //pollSCM ignorePostCommitHooks: true, scmpoll_spec: '* 9-18 * * 1-5'  
	  upstream(upstreamProjects: 'job1', threshold: hudson.model.Result.SUCCESS) // this job will be triggers after success run of Job "job1" 
    
	}
    stages {
        stage('stage1') {
            steps {
                git branch: 'master', credentialsId: '6ab97051-d86e-4ba9-969a-a87d0af82fa9', url: 'https://github.com/anil7411/sample-web.git'
            }
        }
    }
}

==========================================================================================================================================================================

tools directive:-
----------------
A section defining tools to auto-install and put on the PATH. This is ignored if agent none is specified
Allowed: Inside the pipeline block or a stage block.

examples:-
---------
pipeline {
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}

note:- The tool name must be pre-configured in Jenkins under Manage Jenkins → Global Tool Configuration

===============================================================================================================================================================================

input directive:-
----------------
The input directive on a stage allows you to prompt for input, using the input step. The stage will pause after any options have been applied, and before entering the agent block for that stage or evaluating the when condition of the stage. If the input is approved, the stage will then continue. 
Any parameters provided as part of the input submission will be available in the environment for the rest of the stage

Configuration options:-

message:-
-------
Required. This will be presented to the user when they go to submit the input.

id:-
---
An optional identifier for this input. Defaults to the stage name.

ok:-
---
Optional text for the "ok" button on the input form.

submitter:-
---------
An optional comma-separated list of users or external group names who are allowed to submit this input. Defaults to allowing any user.

submitterParameter:-
------------------
An optional name of an environment variable to set with the submitter name, if present.

parameters:-
----------
An optional list of parameters to prompt the submitter to provide. See parameters for more information.

examples:-
---------

pipeline {
    agent any
    stages {
        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
}
========================================================================================================================================================================================================================
========================================================================================================================================================================================================================

Git+Maven+Nexus+Tomcat_Deploy Example:-
=======================================

pipeline {
    agent any
    tools {
        maven "Maven3.6" // Name "Maven3.6 should be conigured in managed jenkins ----> Global Configuration
    }
    parameters {
		string(name: 'BRANCH', defaultValue: '', description: 'Enter Branch Value')
        booleanParam(name: 'Service',defaultValue: false, description: 'Enable Service')
	}
    stages {
        stage('SCM Checkout') {
           steps {
              git branch: 'master', credentialsId: '6ab97051-d86e-4ba9-969a-a87d0af82fa9', url: 'https://github.com/anil7411/sample-web.git'
           }
        }
		stage('mvn build') {
           steps {
              sh "mvn clean package"
           }
        }
	    stage('Nexus Deploy') {
            steps {
               nexusPublisher nexusInstanceId: 'nexus3', nexusRepositoryId: 'app-release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/sample-webapp-1.0.war']], mavenCoordinate: [artifactId: 'app', groupId: 'com.app', packaging: 'war', version: '${BUILD_NUMBER}']]]
            }
        }
      	stage('tomcat deploy') {
		  when {
		   expression { params.BRANCH == 'master' && params.Service == true } 
		   }
           steps {
              deploy adapters: [tomcat8(credentialsId: '1abbc39b-1205-4cf7-bd0b-8b4068d3c0e2', path: '', url: 'http://34.229.232.43:8080/')], contextPath: 'app', war: 'target/sample-webapp-1.0.war'
           }
        }
    }
}
