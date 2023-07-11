pipeline {

    agent any
/*
	tools {
        maven "maven3"
    }
*/
    environment {
        registry = "saikumar313/nodeapp"
        registryCredential = 'dockerhub'
    }

    stages{

        
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
   
            }
        }

        stage ('Build App Image '){
            steps {
                script {
                    dockerImage = docker.build registry + ":V$BUILD_NUMBER"
                }
            }
        }

        stage ('Upload Image') {
            steps {
                script{
                    docker.withRegistry('',registryCredential) {
                        dockerImage.push("V$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage ('Remove Unused Docker Image') {
            steps {
                sh "docker rmi $registry:V$BUILD_NUMBER"
            }
        }

    }

}
