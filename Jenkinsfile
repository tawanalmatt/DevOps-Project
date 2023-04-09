pipeline{
    agent any
    stages{        
        stage("Git checkout"){
            steps{
                echo "====++++executing checkout++++===="
                git 'https://github.com/njokuifeanyigerald/tawana.git'
            }
            post{
                success{
                    echo "====++++succesfully checked out git repo++++===="
                }
                failure{
                    echo "====++++checkout execution failed++++===="
                }
        
            }
        }
        stage("UNIT Testing"){
            steps{
                echo "====++++executing UNIT Testing++++===="
                sh 'mvn test'
            }
            post{
                success{
                    echo "====++++UNIT Testing executed successfully++++===="
                }
                failure{
                    echo "====++++UNIT Testing execution failed++++===="
                }
        
            }
        }
        stage("Integration testing"){
            steps{
                echo "====++++executing Integration testing++++===="
                sh 'mvn verify -DskipUnitTests'
            }
            post{
                success{
                    echo "====++++Integration testing executed successfully++++===="
                }
                failure{
                    echo "====++++Integration testing execution failed++++===="
                }
        
            }
        }
        stage("Maven Build"){
            steps{
                echo "====++++executing Maven Build++++===="
                sh 'mvn clean install '
            }
            post{
                success{
                    echo "====++++Maven Build executed successfully++++===="
                }
                failure{
                    echo "====++++Maven Build execution failed++++===="
                }
        
            }
        }
        
        stage("Docker Image Build"){
            steps{
                echo "====++++executing Docker Image Build++++===="
                script {
                    sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID tawanam/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker image tag $JOB_NAME:v1.$BUILD_ID tawanam/$JOB_NAME:v1.latest"
                }
            }
            post{
                success{
                    echo "====++++Docker Image Build executed successfully++++===="
                }
                failure{
                    echo "====++++Docker Image Build execution failed++++===="
                }
        
            }
        } 

        stage("push image to dockerHub"){
            steps{
                echo "====++++executing push image to dockerHub++++===="


                // withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'dockerhub_cred')]) {
                //     // some block
                //     sh 'docker login -u tawanam -p ${dockerhub_cred}'
                //     sh 'docker image push tawanam/$JOB_NAME:v1.$BUILD_ID'
                //     sh 'docker image push tawanam/$JOB_NAME:v1.latest'
                //     sh 'docker rmi tawanam/$JOB_NAME:v1.$BUILD_ID'
                //     sh 'docker rmi tawanam/$JOB_NAME:v1.latest'
                // }
                withCredentials([string(credentialsId: 'docker_pass', variable: 'dockerhub_cred')]) {
                    // some block
                    sh 'docker login -u tawanam -p ${dockerhub_cred}'
                    sh 'docker image push tawanam/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image push tawanam/$JOB_NAME:v1.latest'
                    sh 'docker rmi tawanam/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker rmi tawanam/$JOB_NAME:v1.latest'
                }
            }
            post{
                
                success{
                    echo "====++++push image to dockerHub executed successfully++++===="
                }
                failure{
                    echo "====++++push image to dockerHub execution failed++++===="
                }
        
            }
        }
        // stage('Docker Deploy') {
        //     steps{
        //         ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
        //     }
        // }    
            
        
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}