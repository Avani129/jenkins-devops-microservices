//SCRIPTED 

//DECLARATIVE 
pipeline {
	agent any
	//agent { docker { image 'maven:3.6.3' } }
	//agent { docker { image 'node:13.10' } }

	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
	    stage ('Checkout') {
			steps {
				sh "mvn --version"
				sh "docker version"
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
		    }
		}
		stage ('Compile') {
			steps {
                sh "mvn clean compile"
			}
		}
		stage ('Test') {
			steps {
                sh "mvn test"
			}
		}
		stage ('Integration Test') {
			steps {

		        sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage ('package') {
			steps {

		        sh "mvn package -DskipTests"
			}
		}
		stage ('Build docker image'){
			steps {
               //"docker build -t avani129/currency-exchange-devops:$env.BUILD_TAG"
			    script {
					dockerImage = docker.build("avani129/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Push docker image') {
			steps{
				script{
				docker.WithRegistry('','dockerhub')
					dockerImage.push();
					dockerImage.push('latest');
				}
			}
		}
		    
	} 
	
	post{
		always{
			echo 'Im awesome. I run always'
		}
	 success {
			echo 'I run when you are successful. '
		}
		failure {
			echo 'I stopped running  when you fail '
		}
	}
}
