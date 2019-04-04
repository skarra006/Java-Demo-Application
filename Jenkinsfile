node{
      
      stage('SCM Checkout'){
         git 'https://github.com/skarra006/Java-Demo-Application'
      }
      
      stage('Build'){
         // Get maven home path and build
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "${mvnHome}/bin/mvn package -Dmaven.test.skip=true"
      }       
       
      stage ('Test'){
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "${mvnHome}/bin/mvn verify; sleep 3"
      }
      
      stage('Build Docker Image'){
         sh 'docker build -t skarra006/javademoapp_$JOB_NAME:$BUILD_NUMBER .'
      }  
   
      stage('Publish Docker Image'){
            def dockerpwd = "123Sravya@"
        // withCredentials([(credentialsId: 'dockerpwd', variable: '123Sravya@')]) {
              sh "docker login -u skarra006 -p dockerpwd"
         }
        sh 'docker push skarra006/javademoapp_$JOB_NAME:$BUILD_NUMBER'
        sh "sed -i.bak 's/#BUILD-NUMBER#/$BUILD_NUMBER/' deployment.yaml"
        sh "sed -i.bak 's/#JOB-NAME#/$JOB_NAME/' deployment.yaml"
      }
         
      // ********* For Azure Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials(string[(credentialsId: 'k8pwd', variable: 'Pwc@12345689')]) {
          sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no pwcuser@52.163.94.232" 
          sh "sshpass -p ${k8PWD} scp -r deployment.yaml pwcuser@52.163.94.232:/home/pwcuser" 
          sh "sshpass -p ${k8PWD} ssh  -o StrictHostKeyChecking=no pwcuser@52.163.94.232 ${k8Apply}"
         }
       }
} 
  
       
    
      /* 
      // ********* For AWS Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials([string(credentialsId: 'k8pwdrajni', variable: 'k8PWD')]) {
             sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no devops@54.196.52.131"  
             sh "sshpass -p ${k8PWD} scp -r deployment.yaml devops@54.196.52.131:/home/devops" 
             sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no devops@54.196.52.131 ${k8Apply}"
         }
       }
        
  }
  */
