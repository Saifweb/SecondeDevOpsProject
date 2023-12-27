pipeline{
    agent{
        label "Jenkins-slave"
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    // define paramaters :
    environment{
        // the app name should be a lowercase otherwise the pipline will return an error 
        APP_NAME="java_app"
        RELEASE="1.0.0"
        DOCKER_USER="saifbenhmida1420"
        DOCKER_PASS="dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN= credentials("JENKINS_API_TOKEN")
    }
    stages{
        // ensures that there are no residual artifacts or files from previous build
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        //get the code from github repo!!
        stage("Checkout from the SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Saifweb/SecondeDevOpsProject.git'
                }
        }
        // build using maven
        // sh mean shell script 
        stage("Build the application"){
            steps {
                sh "mvn clean package"
            }
        }
        // test using maven
        // sh mean shell script 
        stage("Test the application"){
            steps {
                sh "mvn test"
            }
        }
        // sonaranalysis using maven cmd
       stage("SonarQube Analysis"){
           steps {
	           script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                            sh "mvn sonar:sonar"
                    }
	           }	
           }
       }
       // Wait for the quality gate and continue even if it fails
        stage("Quality Gate"){
           steps {
	           script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
	           }	
           }
       }
       // you should  create your docker file in the project before using this step 
        stage("Build and Push Docker Image"){
            steps {
                script {
                    //build the image
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    // push the image
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
       }
       // scan the latest docker image using Trivy
        stage("Trivy Scan") {
           steps {
               script {
	            sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image saifbenhmida1420/java_app:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
               }
           }
       }
       // for space Optimization we are going to clean up the local copies of those images on the Jenkins agent  ( they are already pushed , builded and scaned on the dockerHub)
       stage ('Cleanup Artifacts') {
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }
       stage ('Triger Cd Pipline') {
        steps{
                script {
                    sh "curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'http://34.71.214.45:8080/job/gitops-register-app-cd/buildWithParameters?token=gitops-token'"
                }
        }
       }
       
    }
}