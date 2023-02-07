
pipeline {
    agent any
    tools {
      maven 'MAVEN'
    }
    stages {
      stage('Git checkout') {    //Getting the source code from my github repo 'main' branch
           steps {
               git branch: 'master', url: 'https://github.com/Jeevasanna/hello-world-war.git'
           }
       }
//        stage('OWASP-Dependency-Check') { 
//             steps {
//                  dependencyCheck additionalArguments: '--scan /var/lib/jenkins/workspace/${JOB_NAME} --format ALL --disableYarnAudit', 
//                  odcInstallation: 'owasp-dependency-check'
//                  dependencyCheckPublisher pattern: '**/dependency-check-report.xml', unstableNewCritical: 1, unstableNewHigh: 2, unstableTotalCritical: 1, unstableTotalHigh: 2
//            }
//        } 
       stage('Build artifact') {     //This will compile and generate a war file as a package for my java application
            steps {
                 sh 'mvn clean package'
               }
           }
//        stage('sonar Analysis') {    //Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project
//             steps{
//                 withSonarQubeEnv('sonarqube') {
//                    sh 'mvn clean verify sonar:sonar \
//                     -Dsonar.projectName=pipeline-demo-java-application-1 \
//                     -Dsonar.projectKey=java-project \
//                     -Dsonar.login=squ_6cee02bd03f5655e79fde1ecbcc76b18e95e2262 \
//                     -Dsonar.host.url=http://43.204.38.67:9000' 
                        
//                 }
//            }
//        }
// //        stage('Quality Gate Analysis'){    //its for quality policy in our org & tells whether my project is ready for release
// //             steps{
// //                     waitForQualityGate abortPipeline: true 
// //            }
// //        } 
// //        stage("Quality Gate") {
// //             steps {
// //                 timeout(time: 1, unit: 'HOURS') {
// //                   waitForQualityGate abortPipeline: true
// //                 }
// //            }
// //        }
//        stage('Upload war to Nexus'){     //This will push the already generated war file to a prescribed folder in Nexus repository manager 
//             steps{
//                 nexusArtifactUploader artifacts: [    //using this plugin, am uploading my artifact to nexus , got this info from snippet generator
//                     [
//                         artifactId: 'java-web-app',   //got from pom.xml
//                         classifier: '', 
//                         file: 'target/java-web-app-1.0.0.war',   //jenkins workspace war file generated location
//                         type: 'war'
                    
//                     ]
//                 ], 
//                 credentialsId: 'nexus3',     //nexus id , where my username & pwd defined
//                 groupId: 'com.mt',                 // got from pom.xml
//                 nexusUrl: '43.204.22.243/:8081',     //my nexusurl with public-ip, bcoz my nexus and jenkins are not in same network
//                 nexusVersion: 'nexus3', 
//                 protocol: 'http', 
//                 repository: 'pipeline-demo-java-application-release',    //my repo name in nexus
//                 version: '1.0.0'
//             }    
//         }
//            stage('push nexus artifact'){
//                steps {
//                    sh 'mvn clean deploy'
//                }
//            }
//         stage('ansible deployement') {
//             steps {
//               ansiblePlaybook credentialsId: 'ansible-deployment', disableHostKeyChecking: true, installation: 'ansible', inventory: 'hosts.inv', playbook: 'tomcat-installation.yaml'
//             }      
//         }
        
           stage('build docker image') {
               steps {
                   sh 'docker build -t jeevavijayanand/javaapp:latest
               }
           }
//            stage('DockerHUB LOGIN & push image') {
//                 steps {
//                       withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'dockerhubcredentials')]) {
//                       sh "docker login -u account1996 -p ${dockerhubcredentials}"  
//                    }
//                       sh 'docker push account1996/java:1'
//                }
//            }
      }
}  
