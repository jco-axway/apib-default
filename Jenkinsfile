pipeline {
  agent any
   stages {
    stage('Clone API Builder project') {
      steps {
        git "${params.project_repo}"
      }
    }
    stage('Build Docker image') {
      steps{
        script {
           image = docker.build params.docker_repo + ":latest"
          
        }
      }
    }
    stage('Push Docker Image') {
      steps{
        script {
          docker.withRegistry( '', params.credential_id ) {
            image.push()
          }
        }
      }
    }
    stage('Run Docker Container') {
      steps{
        sh "ssh ${params.deploy_user}@${params.deploy_ip} docker run -d --name myproject  -p 8080:8080 $docker_repo:latest"
        sh "ssh ${params.deploy_user}@${params.deploy_ip} docker ps"
      }
    }
    stage('Clean up') {
      steps{
        sh "docker rmi ${params.docker_repo}:latest"
        deleteDir()
      }
    }
  }
}
