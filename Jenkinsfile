// node{
     
//     stage('SCM Checkout'){
//         git url: 'https://github.com/MRaju2022/java_web_docker_app.git',branch: 'master'
//     }
    
//     stage(" Maven Clean Package"){
//       def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
//       def mavenCMD = "${mavenHome}/bin/mvn"
//       sh "${mavenCMD} clean package"
      
//     } 
    
    
//     stage('Build Docker Image'){
//         sh 'docker build -t dockerhandson/java-web-app .'
//     }
    
//     stage('Push Docker Image'){
//         withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
//           sh "docker login -u dockerhandson -p ${Docker_Hub_Pwd}"
//         }
//         sh 'docker push dockerhandson/java-web-app'
//      }
     
//       stage('Run Docker Image In Dev Server'){
        
//         def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockerhandson/java-web-app'
         
//          sshagent(['DOCKER_SERVER']) {
//           sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.72 docker stop java-web-app || true'
//           sh 'ssh  ubuntu@172.31.20.72 docker rm java-web-app || true'
//           sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
//           sh "ssh  ubuntu@172.31.20.72 ${dockerRun}"
//        }
       
//     }
// }


node{
    stage('clone repo'){
       git branch: 'main', credentialsId: 'GITHUB-CREDENTIALS', url: 'https://github.com/MRaju2022/java_web_docker_app.git'
    }
    stage('Maven Build'){
        def mavenHome = tool name: "Maven-3.8.6", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    stage('Build Image'){
        sh 'docker build -t mraju25/javawebapp1 .'
    }
    stage('Push Image'){
        withCredentials([string(credentialsId: 'DOCKER', variable: 'DOCKER')]) {
          sh 'docker login -u mraju25 -p ${DOCKER}'
        }
        sh 'docker push mraju25/javawebapp1'
    }
    stage('Deploy'){
        
        kubernetesDeploy(
	       configs: 'java-web-app-deployment.yml',
	       kubeconfigId: 'KUBECONFIG'
	  )
    }
}
