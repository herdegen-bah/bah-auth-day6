node {
    stage ("Checkout AuthService"){
        git branch: 'main', url: 'https://github.com/herdegen-bah/bah-auth-day6.git'
    }
    
    stage ("Gradle Build - AuthService") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle Bootjar-Package - AuthService") {
        sh 'gradle bootjar'
    }
    
    stage ("Delete Previous Env - AuthApi){
    	sh 'kubectl delete deployment auth-day7 || true'
    	sh 'kubectl delete service auth-day7 || true'
    	sh 'docker rmi auth-day7 || true'
    }
	
	stage ("Containerize the app-docker build - AuthApi") {
        sh 'docker build --rm -t auth-day7:v1.0 .'
    }
    
    stage ("Inspect the docker image - AuthApi"){
        sh "docker images auth-day7:v1.0"
        sh "docker inspect auth-day7:v1.0"
    }
    
    stage ("Run Docker container instance - AuthApi"){
        sh "docker run -d --rm --name event-auth -p 8081:8081 event-auth:v1.0"
    }
    

    stage('User Acceptance Test - AuthApi') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	    stage('Deploy to Kubenetes cluster - AuthApi') {
		  sh "kubectl create deployment auth-day7 --image=auth-day7:v1.0"
    		sh "kubectl set env deployment/auth-day7 API_HOST=\$(kubectl get service/data-day7 -o jsonpath='{.spec.clusterIP}'):8080"
    		sh "kubectl expose deployment auth-day7 --type=LoadBalancer --port=8081"
	    }
	  }
    }
    stage("Production Deployment View"){
    	sh "kubectl get deployments"
    	sh "kubectl get pods"
    	sh "kubectl get services"
    }
}
