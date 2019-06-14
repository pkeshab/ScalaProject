pipeline {
    agent any
	environment{
	   registry="kpandeydocker/scalaproject"
	   registryCredential='docker-credentials'
	   dockerImage=''
	}
    stages {
        stage('TEST THE CODE AND CREATE THE ARTIFACTS'){
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
                echo "$WORKSPACE"
                //sh label: '', script: 'echo "$GIT_URL"'
                
                sh label: '', script: 'java -jar ${WORKSPACE}/target/scala-2.12/HelloWorld-assembly-0.1.jar'
                //archiveArtifacts artifacts: 'target/scala-2.12/*', onlyIfSuccessful: true
                sh label: '', script: 'ARTIFACT_VALUE=$WORKSPACE/target/scala-2.12/HelloWorld-assembly-0.1  && echo $ARTIFACT_VALUE'
               //echo " $ARTIFACT_VALUE" > application.jar

            
            }
        }
      
            

        stage('UPLOAD THE CREATED ARTIFACTS INTO NEXUS3 REPOSITORY'){
            steps{
                script {
            echo "$WORKSPACE"
            //sh label: '', script: 'ls ${WORKSPACE}/target/scala-2.12' 
			withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'nexus-credentials',
usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "curl -v -u $USERNAME:$PASSWORD --upload-file $WORKSPACE@2/target/scala-2.12/HelloWorld-assembly-0.1.jar  http://10.1.100.158:8081/repository/releases/LOVEN/$BUILD_NUMBER/1.0/$BUILD_NUMBER-1.0.jar"
            }
			}
            }
        }
        stage('CREATE THE DOCKERIMAGE OF THE LATEST ARTIFACTS FROM NEXUS3 '){
            steps{
		    script{
		       dockerImage = docker.build registry + ":$BUILD_NUMBER"
		    }
          
            
            }
        }
		stage('PUSH THE DOCKER IMAGE INTO DOCKER REGISTRY'){
			steps{
			    script {
          			docker.withRegistry( '', registryCredential ) {
            			dockerImage.push()
          }
				    
        }
			}

			}
        }
    
    
    
    } 
