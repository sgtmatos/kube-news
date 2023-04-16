pipeline {
    agent any

    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("armtsandre/kube-news${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

         stage ('Push Docker Image') {
             steps {
                 script {
                     docker.withRegistry('https://registry.hub.docker.com', 'login.docker') {
                         //dockerapp.push('latest')
                         dockerapp.push("${env.BUILD_ID}")
                     }
                 }
             }
         }


         stage ('Deploy Kubernetes') {
             environment {
                tag_version = "${env.BUILD_ID}"
             }
             steps {
                 withKubeConfig ([credentialsId: 'doc.kubeconfig']) {
                     sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                     sh 'kubectl apply -f ./k8s/deployment.yaml' // --insecure-skip-tls-verify'
                 }
             }
         }
    }

<<<<<<< HEAD
}
=======
}
>>>>>>> 1e7f0b8b08c41728bd9d45328e836553b867f6ee
