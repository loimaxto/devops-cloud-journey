## 1 Jenkins syntax
1. pipeline: top-level
2. agent : where to execute
3. stages:
	1. stage - step

```groovy

pipeline {   
    agent any
    parameters {
	    // these are global var
	    string(name: 'VERSION', defaultValue: '', description:'ver to deploy')
	    choice(name:'ENV', choices:["dev", "test"], description:'')
	    booleanParam(name: 'executeTests', defaultValue: true, description:'')
    }
    tools {
        maven 'maven3.9' //must map with defined tools in jenkins
    }
    environment {
	    NEW_VERSION = '1.3.0'
	    SERVER_CREDENTIALS = credentials('server-credentials')
    }
    stages {
        stage("build image") {
            steps {
               echo "building version ${NEW_VERSION}"
            }
        }
        stage("test") {
	        when {
		        expression {
			        //BRANCH_NAME == 'DEV' || BRANCH == 'MASTER'
			        params.executeTests
		        }
	        }
	        steps {
	        }
        }

        stage("deploy") {
            steps {
                env.ENV = input message: "Select the environment to deploy"/
                echo 'deploying app ${params.VERSION}'
            }
        }
        stage("demo") {
	            dir(path: 'app') {
						sh 'npm test'
					}
        }              
    }
    
    post {
	    always {} // always run
	    success {}
	    failure {}
    }
} 

```

## 2 Jenkinsfile template

### Deploy to Dockerhub

**Jenkinsfile**
```groovy
def gv

pipeline {   
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    gv.buildJar()

                }
            }
        }

        stage("build image") {
            steps {
                script {
                    gv.buildImage()
                }
            }
        }

        stage("deploy") {
				input {
				message "Select the environment to deploy"
				ok "Done"
				parameters{
					  choice(name:'ENV', choices:["dev",], description:'deploy environment')
				 }
            }

            steps {
                script {
                  gv.deployApp()
                }
            }
        }               
    }
} 

```

**script.groovy**
```groovy
def buildJar() {
    echo 'building the application...'
    sh 'mvn package'
}

def buildImage() {
    echo "building the docker image..."
    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        sh 'docker build -t loimaxto/test-repo:jenkins-1 .'
        sh 'echo $PASS | docker login -u $USER --password-stdin'
        sh 'docker push nanatwn/demo-app:jma-2.0'
    }
}

def deployApp() {
    echo 'deploying the application...'
}

return this

```

## 3 Multi-branch pipeline

### Best practice
- One shared Jenkinsfile for all branches ( store in master branch)