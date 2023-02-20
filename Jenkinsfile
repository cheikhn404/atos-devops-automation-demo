pipeline {
    agent any
    tools{
        maven 'Maven-3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/cndiaye-dev/atos-devops-automation-demo']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t cheikhn414/atos-devops-automation-demo .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: '302964a2-0432-4c5b-a3af-eae839041960', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u cheikhn414 -p ${dockerhubpwd}'
                    }
                    sh 'docker push cheikhn414/atos-devops-automation-demo'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deployment-service.yaml', kubeConfig: [path: ''], kubeconfigId: 'k8sconfigpwd', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://'])
                }
            }
        }
    }
}