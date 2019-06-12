pipeline {
    agent any
    stages {
        stage('Test and package'){
            agent {
        docker {
            image 'hseeberger/scala-sbt'
        }
    }

            steps {
                echo 'SBT test and package..'
                sh "sbt test"
               // sh label: '', script: 'java -version'
                sh label: '', script: 'sbt clean assembly'
                sh label: '', script: 'ls ${WORKSPACE}/target/scala-2.12'
                //echo "$BUILD_NUMBER"
                //sh label: '', script: 'echo "$JOB_NAME"'
                //sh label: '', script: 'git branch'
                //echo "$WORKSPACE"
                //sh label: '', script: 'echo "$GIT_URL"'
                
                sh label: '', script: 'java -jar ${WORKSPACE}/target/scala-2.12/HelloWorld-assembly-0.1.jar'
            
            }
        }
      
            
            stage('Upload in nexus repo'){
                steps {
           nexusArtifactUploader artifacts: [[artifactId: 'diwo-artifact_amx_$BUILD_NUMBER', classifier: 'classifier', file: '/var/jenkins_home/workspace/$JOB_NAME_master@2/target/scala-2.12/*.jar', type: 'jar']], credentialsId: 'nexus-credentials', groupId: 'mygroupid', nexusUrl: '10.1.100.158:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'repository-example', version: 'v3'
             

                       }
                
            }
        stage('Build the docker image'){
            steps{
            sh 'docker ps'
            sh 'docker build -t scalasampleimage:latest .'
            
            }
        }
          
        }
    
    
    
    } 
