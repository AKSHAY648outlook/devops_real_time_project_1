pipeline {
    agent any
    environment {
      PATH = "$PATH:/usr/share/maven"
    }
    
    stages {

        stage('CLEAN WORKSPACE') {
            steps {
                cleanWs()
            }
        }

        stage('CODE CHECKOUT') {
            steps {
                git 'https://github.com/AKSHAY648outlook/devops_real_time_project_1.git'
            }
        }
    } 
}  
