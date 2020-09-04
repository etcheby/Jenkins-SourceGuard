 pipeline {
      agent any
      environment {
           SG_CLIENT_ID = credentials("SG_CLIENT_ID")
           SG_SECRET_KEY = credentials("SG_SECRET_KEY")
           registry = "https://registry.hub.docker.com"
       
           dockerImage = 'etcheby84/vulnerablewebapp'
        }
  stages {
          
         stage('Clone Github repository') {
            
    
           steps {
              
             checkout scm
           
             }
  
          }
    stage('SourceGuard Code Scan') {   
       steps {   
                   
         script {      
              try {
         
               
            
                sh 'chmod +x sourceguard-cli' 

                sh './sourceguard-cli --src .'
           
               } catch (Exception e) {
    
                 echo "Request for Approval"  
                  }
              }
            }
         }
           
           
          stage('Docker image Build and scan prep') {
             
            steps {

              sh 'sudo docker build -t etcheby84/vulnerablewebapp .'
              sh 'sudo docker save etcheby84/vulnerablewebapp -o vulnerablewebapp.tar'
              
             } 
           }
       stage('SourceGuard Container Image Scan') {   
          steps {
            script {
               try {     
           
         
                    sh './sourceguard-cli --img sg.tar'
                    } catch (Exception e) {
    
                 echo "Image scanning is BLOCK and recommend not using the source code"  
                  }
                }
              }
            
            }
            
        
        
  } 
}
