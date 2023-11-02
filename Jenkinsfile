pipeline {
    agent any
    environment {

        GIT_REPO = "https://github.com/aakkiiff/demo_config.git" 
        APP_NAME = "demo_frontend"

    }

    stages{

        stage('CLEANUP WORKSPACE'){
            steps{
                script{
                    cleanWs()
                }
            }
        }

        stage("CHECKOUT GIT REPO"){
            steps{
                git "${GIT_REPO}"
            }
        }

        stage("UPDATE K8S DEPLOYMENT FILES AND PUSH TO THE GIT REPO"){
            steps{

                sh 'cat ./k8s/deployment.yaml'
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' ./k8s/deployment.yaml"
                sh 'cat ./k8s/deployment.yaml'

                sh 'git config --global user.email jackakif@gmail.com'
                sh 'git config --global user.name aakkiiff'
                sh 'git add ./k8s/'
                sh "git commit -m 'Updated deployment files to ${IMAGE_TAG}'"

                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                    sh 'git push https://$uname:$pass@github.com/aakkiiff/demo_config.git master'
                }
            }
        }
    }
}