pipeline {
  agent any
	environment{ 
		mavenHome = tool 'myMaven'
		PATH = "$mavenHome/bin:$PATH"
	}

                          
              stages{
              stage('Checkout') {
                             steps{   
                             sh 'mvn --version'
                             sh 'docker version'
			     sh 'gcloud version'
			    	     
			    
                             echo "Build" 
                             echo "PATH - $PATH"
                             echo "BUILD_NUMBER - $env.BUILD_NUMBER"
                             echo "BUILD_ID - $env.BUILD_ID"
                             echo "JOB_NAME - $env.JOB_NAME"
                             echo "BUILD_TAG- $env.BUILD_TAG"
                             echo "BUILD_URL- $env.BUILD_URL"
                             }
              }
  stage('Test') {
            steps { 
                echo "Test"
            }
        }
                             stage('Package') {
				     agent {
                label 'java-docker-slave'
                   }
            steps { 
               echo "Started creating jar file"
                                              sh "mvn clean install -DskipTests"
                                              echo "Completed creating jar file"
            }
        }

              stage ('Build Docker Image')
                             {
                                           steps{
                                                          script{
                                                                        dockerImage=docker.build("rachanamadhav/currency-exchange:${env.BUILD_TAG}");
                                                          }
                                           }
                             }             
              stage ('Push Docker Image')
              {steps{
                             script{
                             docker.withRegistry('','dockerhub')         
                                           {             dockerImage.push();
                                                   dockerImage.push('latest'); 
                                           }
                       }
              }}
              stage('Maven Test') {
                             steps{   
                                           sh 'mvn test'
                             }
              }
              
              }             
              post
              {
                             success{
                                           echo "Build was successful"
                             }
                             always{
                                           echo "finished somehow"
                             }
              }
}
