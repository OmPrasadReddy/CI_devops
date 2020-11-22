node{
    stage('Git Checkout'){
        git credentialsId: '533c9956-abd7-4500-b397-3c79ead405a1', url: 'https://github.com/OmPrasadReddy/CI_devops.git'
    }
    stage('MVN pacakge'){
        def mvnHome = tool name: 'M2_HOME', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh label: '', script: "${mvnCMD} clean package"
    }
    stage('Build a Docker Image'){
        sh 'docker build -t omprasadreddy/my-app:2.0.0 .'
        
    }
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhub')]) {
            sh "docker login -u omprasadreddy -p ${dockerhub}"
        }    
        sh 'docker push omprasadreddy/my-app:2.0.0'
    }
    stage('Run Conatiner on Dev server'){
        def dockerRUN = 'docker run -p 8080:8080 -d --name my-app omprasadreddy/my-app:2.0.0'
        sshagent(['88a8d9df-2a9b-46a1-842e-c09f81ca869c']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.86.43 ${dockerRUN}"
        }
    }
    
}
