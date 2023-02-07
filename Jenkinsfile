pipeline {
  agent any
  // tools{
  //   maven 'Maven 3.9.0'
  //   jdk 'jdk11'
  // }
  environment {     
    DOCKERHUB_CREDENTIALS= credentials('docker')     
  }
  stages {
    stage('checkout') {
      steps {                 
        git credentialsId: 'one', url: 'git@github.com:nishantindorkar/CI-CD-using-Docker.git'
        }
    }            
    stage('Execute Maven') {
      steps { 
        // sh 'sudo apt-get update -y'
        // sh 'sudo apt-get install maven -y'           
        sh 'mvn package'             
      }
    }
    stage('Docker Build and Tag') {
      steps {
        // sh 'sudo apt-get update -y'
        // sh 'sudo apt-get install -y docker.io'
        // sh 'sudo apt-get install -y docker-compose'
        sh 'sudo docker build -t samplewebapp:latest .' 
        sh 'sudo docker tag samplewebapp nishantindorkar/samplewebapp:latest'
        //sh 'docker tag samplewebapp nishantindorkar/samplewebapp:$BUILD_NUMBER'
        echo 'Build Image Completed'
        }
    }
    stage('Publish image to Docker Hub') {
      steps {
        //withCredentials([string(credentialsId: 'dockersecrettext', variable: 'dockerconatiner')]) {
        //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login --username nishantindorkar --password-stdin'                 
	      withDockerRegistry(credentialsId: 'docker', url: 'https://hub.docker.com/repository/docker/nishantindorkar/jenkins-cicd'){
        echo 'Login Completed'
        sh  'sudo docker push nishantindorkar/samplewebapp:latest'
        //sh  'docker push nishantindorkar/samplewebapp:$BUILD_NUMBER' 
        echo 'Push Image Completed'
        }
      }
    }
    stage('Run Docker container on Jenkins Agent') {
      steps{
        sh "docker run -d -p 8003:8080 nikhilnidhi/samplewebapp"
      }
    }
    // stage('Run Docker container on remote hosts') {
    //   steps {
    //     //sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 nikhilnidhi/samplewebapp"
    //   }
    // }
  }
}
      
