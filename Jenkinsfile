
pipeline {
    agent any
    tools {
      maven 'MAVEN'
    }
    environment {
      AWS_ACCOUNT_ID= "681424868466"
      AWS_DEFAULT_REGION="ap-south-1"
      IMAGE_REPO_NAME= "vijay-ecr"
      IMAGE_TAG= "latest"
      REPOSITORY_URI= "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
      stage('Git checkout') {    //Getting the source code from my github repo 'main' branch
           steps {
               git branch: 'master', url: 'https://github.com/Jeevasanna/hello-world-war.git'
           }
       }
       stage('OWASP-Dependency-Check') { 
            steps {
                 dependencyCheck additionalArguments: '--scan /var/lib/jenkins/workspace/${JOB_NAME} --format ALL --disableYarnAudit', 
                 odcInstallation: 'owasp-dependency-check'
                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml', unstableNewCritical: 1, unstableNewHigh: 2, unstableTotalCritical: 1, unstableTotalHigh: 2
           }
       } 
       stage('Build artifact') {     //This will compile and generate a war file as a package for my java application
            steps {
                 sh 'mvn clean package'
               }
           }
       stage('sonar Analysis') {    //Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project
            steps{
                withSonarQubeEnv('sonarqube') {
                   sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectName=pipeline-demo-java-application-1 \
                    -Dsonar.projectKey=java-project \
                    -Dsonar.login=squ_6cee02bd03f5655e79fde1ecbcc76b18e95e2262 \
                    -Dsonar.host.url=http://43.204.38.67:9000' 
                        
                }
           }
       }

       stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                  waitForQualityGate abortPipeline: true
                }
           }
       }
           stage('push nexus artifact'){
               steps {
                   sh 'mvn clean deploy'
               }
           }
        stage('ansible deployement') {
            steps {
              ansiblePlaybook credentialsId: 'ansible-deployment', disableHostKeyChecking: true, installation: 'ansible', inventory: 'hosts.inv', playbook: 'tomcat-installation.yaml'
            }      
        }
        
           stage('Pushingto ECR') {
               steps {
                  sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 681424868466.dkr.ecr.ap-south-1.amazonaws.com" 
                  sh "docker build -t vijay-ecr ."
                  sh "docker tag vijay-ecr:latest 681424868466.dkr.ecr.ap-south-1.amazonaws.com/vijay-ecr:latest"
                  sh "docker push 681424868466.dkr.ecr.ap-south-1.amazonaws.com/vijay-ecr:latest"                   
               }
           }
        

        
              stage('K8S Deploy') {
                   steps{
                          sh 'aws eks update-kubeconfig --name vijay --region ap-south-1'
                          sh 'kubectl apply -f deployment.yml'
                       
                   }
              } 
      }
 } 
