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
        
        stage('MODIFIED IMAGE TAG') {
            steps {
                sh '''
                   sed "s/image-name:latest/$JOB_NAME:v1.$BUILD_ID/g" playbooks/dep_svc.yml
                   sed -i "s/image-name:latest/$JOB_NAME:v1.$BUILD_ID/g" playbooks/dep_svc.yml
                   sed -i "s/IMAGE_NAME/$JOB_NAME:v1.$BUILD_ID/g" webapp/src/main/webapp/index.jsp
                   '''
            }            
        }
        stage('BUILD') {
            steps {
                sh 'mvn clean install package'
            }
        }  
        stage('SONAR SCANNER') {
            environment {
            sonar_token = credentials('SONAR_TOKEN')
            }
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectName=$JOB_NAME \
                    -Dsonar.projectKey=$JOB_NAME \
                    -Dsonar.host.url=http://172.31.13.240:9000 \
                    -Dsonar.token=$sonar_token'
            }
        } 
       stage('COPY JAR & DOCKERFILE') {
            steps {
                sh 'ansible-playbook playbooks/create_directory.yml'
            }
        }
        stage('DEPLOYMENT ON EKS') {
            steps {
                sh 'ansible-playbook playbooks/create_pod_on_eks.yml \
                    --extra-vars "JOB_NAME=$JOB_NAME"'
            }            
        }          


        

        
        
        
    } 
}  
