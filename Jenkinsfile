pipeline {
    agent any
    stages {
      stage('checkout') {
           steps {             
                git credentialsId: '01b0261b-cbbc-4129-afa7-635e874f892d', url: 'https://github.com/Geetha699/CI-CD-using-Docker.git'
          }
        }
      stage('Execute maven') {
           steps {
                sh 'mvn package'             
          }
        }
         stage('War-file-Copy') {
            steps {
               sshagent(['1234']) {
               sh "scp -r -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Jenkins-dockerDemo/[!.]* ubuntu@172.31.23.168:~/"
             }
         }
      }
        stage ('Docker Image Build') {
            steps {
            sshagent (['1234']){
               sh 'ssh ubuntu@172.31.23.168 "bash /home/ubuntu/scripts/dockerbuild.sh"'
               }         
            }            
        }
            
  stage('Publish image to Docker Hub') {
    
            steps {
            sshagent (['1234']){    
         sh 'ssh ubuntu@172.31.23.168 "bash /home/ubuntu/scripts/dockerpush.sh"'
        }
            }
  }
  stage ('Deploy') {
         
            steps {
		 sshagent(['1234']) {
                        sh 'ssh ubuntu@172.31.23.168 "sh /home/ubuntu/scripts/dockerrun.sh"'
          }
        }   
      }


}
}


