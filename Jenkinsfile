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

    stage('run dockerfile') {
      steps {
        sh 'docker build -t tulsiramdocker/financeimg:v1 .'
      }
    }
	
	stage('Docker Login') {
      steps {
        withCredentials([string(credentialsId: 'dockerhubpasswd', variable: 'dockerhubpasswd')]) {
		sh 'docker login -u tulsiramdocker -p ${dockerhubpasswd}'
		}	
      }
    }
	
	stage('Docker Push') {
      steps {
		sh 'docker push tulsiramdocker/financeimg:v1'
		}	
      }
	
    stage('Invoke Ansible Playbook') {
      steps {
        ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
      }
    }
  }
}
