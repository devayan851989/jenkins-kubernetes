pipeline{
    agent any
    stages {
        stage('Build Maven') {
            steps{
                  checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/devayan851989/jenkins-kubernetes.git'  ]]])

             
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t devayanthakur/nodejsapp-1.33 .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devayanthakur', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u devayanthakur -p ${dockerhubpwd}'
                 }  
                 sh 'docker push devayanthakur/nodejsapp-1.33'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
        withCredentials([
            string(credentialsId: 'my_kubernetes', variable: 'api_token')
            ]) {
             sh 'kubectl --token $api_token --server https://192.168.103.2:8443  --insecure-skip-tls-verify=true apply -f nodejsapp.yaml '
               }
            }
}
        }
      
    }
 
