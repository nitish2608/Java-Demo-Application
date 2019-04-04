node{
      
      stage('SCM Checkout'){
         git 'https://github.com/nitish2608/Java-Demo-Application'
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
         sh 'docker build -t nitish/javademoapp_$JOB_NAME:$BUILD_NUMBER .'
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: '26!MOODLe', variable: 'dockerPWD')]) {
              sh "docker login -u nitishk2608 -p ${dockerPWD}"
         }
        sh 'docker push rajnikhattarrsinha/javademoapp_$JOB_NAME:$BUILD_NUMBER'
        sh "sed -i.bak 's/#BUILD-NUMBER#/$BUILD_NUMBER/' deployment.yaml"
        sh "sed -i.bak 's/#JOB-NAME#/$JOB_NAME/' deployment.yaml"
      }
          
    
      // ********* For Azure Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials([string(credentialsId: 'k8pwd-nitish', variable: 'k8PWD')]) {
          sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no ubuntu@138.91.122.19" 
          sh "sshpass -p ${k8PWD} scp -r deployment.yaml ubuntu@138.91.122.19:/home/ubuntu" 
          sh "sshpass -p ${k8PWD} ssh  -o StrictHostKeyChecking=no ubuntu@138.91.122.19 ${k8Apply}"
         }
       }
      
                                 }
       
    
      /* 
      // ********* For AWS Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials([string(credentialsId: '26!MOODLe', variable: 'k8PWD-nitish')]) {
             sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no devops@104.211.177.6"  
             sh "sshpass -p ${k8PWD} scp -r deployment.yaml devops@104.211.177.6:/home/devops" 
             sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no devops@104.211.177.6 ${k8Apply}"
         }
       }
        
  }
*/
