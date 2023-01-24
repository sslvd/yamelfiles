pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('J_D')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t sslvd/docker_jenkins:2.0 .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push sslvd/docker_jenkins:2.0'
      }
    }
     stage('Ansible')
    {
      steps{
          ansiblePlaybook credentialsId: 'webservers', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'ansible.yml'
         
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}