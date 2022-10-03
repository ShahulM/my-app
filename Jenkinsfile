node{
   stage('Shahul SCM Checkout'){
     git 'https://github.com/ShahulM/my-app'
   }
   stage('Shahul Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Shahul SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Shahul Build Docker Imager'){
   sh 'docker build -t jjaba/myweb:0.0.2 .'
   }
   stage('Shahul Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u jjaba -p ${dockerPassword}"
    }
   sh 'docker push jjaba/myweb:0.0.2'
   }
   stage('Shahul Nexus Image Push'){
   sh "docker login -u admin -p Admin@123 ubuntunexus.local:8087"
   sh "docker tag jjaba/myweb:0.0.2 ubuntunexus.local:8087/damo:1.0.0"
   sh 'docker push ubuntunexus.local:8087/damo:1.0.0'
   }
   stage('Shahul Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Shahul Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest jjaba/myweb:0.0.2' 
   }
}
}
