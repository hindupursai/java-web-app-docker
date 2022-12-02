node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/hindupursai/java-web-app-docker.git',branch: 'master'
        }
 stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven-3.8.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    stage('Build Docker Image'){
        sh 'docker build -t hindupursai/java-web-app .'
    }
    stage('Push Docker Image'){
    withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
         sh "docker login -u hindupursai -p ${Docker_Hub_Pwd}"
    // some block
    }
        sh 'docker push hindupursai/java-web-app'
    }
    stage('Run Docker Image In Dev Server'){
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app hindupursai/java-web-app'
    sshagent(['Docker_Deployment']) {
    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.171 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.47.171 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.47.171 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.47.171 ${dockerRun}"
       }
}
}
