node{
   stage('SCM Checkout'){
       checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Anuyenugula/my-app.git']])
   }
   stage('Mvn Package'){
     def mvnHome = tool name: 'maven', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
   stage('Build Docker Image'){
     sh 'docker build -t anugaddam/project1:1.0.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerHubPwd')]) {
        sh "docker login -u anugaddam -p ${dockerHubPwd}"
     }
        sh 'docker push anugaddam/project1:1.0.0'
   }
   stage('Run Container on Dev Server'){
        def dockerRun = 'docker run -p 8080:8080 -d --name my-app anugaddam/project1:1.0.0'
        sshagent(['anu']) {
        sh "ssh -o StrictHostKeyChecking=no anu@172.31.37.4 ${dockerRun}"
     }
   }
}
