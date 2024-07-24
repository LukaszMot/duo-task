pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t lukaszm13/duo-deploy-flask:latest -t stratcastor/duo-deploy-flask:v$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push lukasz13/duo-deploy-flask:latest
                docker push lukaszm13/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi lukaszm13/duo-deploy-flask:latest
                docker rmi lukaszm13/duo-deploy-flask:v$BUILD_NUMBER
                ''' 
            } 
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }
}
