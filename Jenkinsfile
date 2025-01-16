pipeline {
  agent any
  stages {
    stage('checkout the code from github') {
      steps {
        git url: 'https://github.com/bhardwajtulsiram/capstone-banking-java-project'
        echo 'github url checkout'
      }
    }
    stage('compile code') {
      steps {
        echo 'starting compiling'
        sh 'mvn compile'
      }
    }
    stage('testing the code') {
      steps {
        sh 'mvn test'
      }
    }
    stage('qa with checkstyle') {
      steps {
        sh 'mvn checkstyle:checkstyle'
      }
    }
    stage('package the code') {
      steps {
        sh 'mvn package'
      }
    }

    stage('Execute Ansible Playbook for Docker Configuration') {
      steps {
        echo 'Executing Ansible Playbook'
        sh '''
        ansible - playbook configure_docker.yml '''
      }
    }
    stage('run dockerfile') {
      steps {
        sh 'docker build -t financeimg .'
      }
    }
    stage('port expose') {
      steps {
        sh 'docker run -dt -p 8091:8091 --name container1 financeimg'
      }
    }
  }
}
