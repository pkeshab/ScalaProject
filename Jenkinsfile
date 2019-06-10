pipeline {
    agent {
        docker {
            image 'hseeberger/scala-sbt'
        }
    }
    stages {
        stage('Compile') {
            steps {
                echo 'SBT compile..'
                sh "sbt compile"
            }
        }
        stage('Test and package'){
            steps {
                echo 'SBT test and package..'
                sh "sbt test"
                sh "sbt package"
            
            }
        }
            
            stage('Upload in nexus repo'){
                steps {
                            nexusArtifactUploader artifacts: [[artifactId: 'artifacts-id', classifier: 'classifier', file: '/var/jenkins_home/workspace/HelloWorld_master/target/scala-2.12/helloworld_2.12-0.1.jar', type: 'jar']], credentialsId: 'nexus-credentials', groupId: 'mygroupid', nexusUrl: '10.1.100.158:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'repository-example', version: 'v3'
                       }
                
            }
          
        }
    } 
