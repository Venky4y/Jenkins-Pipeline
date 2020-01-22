node{
	   stage('SCM Checkout'){
	       git credentialsId: 'git-creds', url: 'https://github.com/Venky4y’
	   }
	   stage('Mvn Package'){
	     def mvnHome = tool name: 'maven-3', type: 'maven'
	     def mvnCMD = "${mvnHome}/bin/mvn"
	     sh "${mvnCMD} clean package"
	   }
	   stage('Build Docker Image'){
	     sh 'docker build -t venky/my-app:2.0.0 .'
	   }
	   stage('Push Docker Image'){
	     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
	        sh "docker login -u venky -p ${dockerHubPwd}"
	     }
	     sh 'docker push venky/my-app:2.0.0'
	   }
	   stage('Run Container on Dev Server'){
	     def dockerRun = 'docker run -p 8080:8080 -d --name my-app venky/my-app:2.0.0'
	     sshagent(['dev-server']) {
	       sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.18.198 ${dockerRun}"
	     }
	   }
	}
