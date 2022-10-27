pipeline { 
    environment { 
        registry = "sammydev3564/docker-project" 
        registryCredential = 'dockerhublogin'
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git 'https://github.com/Sammydev3564/dockerproject.git' 
            }
        } 
        stage('Building image Docker') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image to Dockerhub') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push()
                        dockerImage.push('latest') 
                    }
                }
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        }
        stage('Run container on ECS') { 
            steps { 
                withAWS(region:'us-east-2', credentials:'aws-cred' ) {
               sh 'aws ecs update-service --cluster my-cluster --service newecs-service --task-definition newecs-tg --force-new-deployment'
            }
            }
        }
         
    }
}