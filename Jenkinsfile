pipeline {
    agent {
        docker {
            image 'hseeberger/scala-sbt'
        }
    }
    stages {
        stage('Compile and run') {
            steps {
                echo 'SBT compile..'
                sh "sbt compile"
                sh "sbt run"
            }
        }
        stage('Test and package'){
            steps {
                echo 'SBT test and package..'
                sh "sbt test"
                sh "sbt package"
                echo "$BUILD_NUMBER"
            
            }
        }
            
            stage('Upload in nexus repo'){
                steps {
                            nexusArtifactUploader artifacts: [[artifactId: 'diwo-artifact_amx_$BUILD_NUMBER', classifier: 'classifier', file: '/var/jenkins_home/workspace/HelloWorld_master/target/scala-2.12/myfile', type: 'zip']], credentialsId: 'nexus-credentials', groupId: 'mygroupid', nexusUrl: '10.1.100.158:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'repository-example', version: 'v3'
                       }
                
            }
          
        }
    } 
