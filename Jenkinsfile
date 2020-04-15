pipeline {
   

    agent {

        docker { image 'sourceguard/sourceguard-cli:latest' }

       }
     
     environment {
         
              SG_CLIENT_ID = '5bdd3443-3919-4acc-8212-ed140185bc0d'
              SG_SECRET_KEY = '15c8074c194b4eb8988cfe010309ff78'
              registry = "dhouari/jenkinsSG"
              registryCredential = 'docker_hub'
              dockerImage = ''
             
            }
    
    stages {
    
        stage('Clone Github repository') {
           
           steps {
             
             checkout scm
           
             }
  
          }
        
        
        stage('SourceGuard') {

            steps {

                sh '/sourceguard-cli --src ./'

            }
        }
       
        stage('Initialize'){
       
           
          def dockerHome = tool 'myDocker'
           env.PATH = "${dockerHome}/bin:${env.PATH}"
         }
        
        stage('Docker image Build') {
        /* Using Dockerfile to build the container image*/
            
             steps {
               
                 script {
                    
                
                    
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                     
                     } 
               }
         }

       stage('Push to Docker Registry') {
        
           steps {
                
               script {
                   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                 
                          }
                  }
            }
       
       }
        
    }

}
