pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'MyDockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh """
                docker build . -f dockerfile -t abanobayad/myapp:v1 --network host
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker push abanobayad/myapp:v1
                """
                }
            }
        }
         stage('CD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'MyDockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh """
                kubectl apply -f /var/jenkins_home/workspace/my-app/deploy1.yaml
                kubectl apply -f /var/jenkins_home/workspace/my-app/lb.yaml
                """
                }
            }
        }
    }
}
