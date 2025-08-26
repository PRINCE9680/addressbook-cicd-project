pipeline{
    agent any

    tools {
        maven 'maven3'
    }
    environment {
        APP_NODE = 'ec2-user@54.165.139.224'
    }
    stages{
        stage('Checkout code'){
          steps{
                 git branch:'main', url: 'https://github.com/PRINCE9680/addressbook-cicd-project'
          }
        }
        stage('Build with maven'){
          steps{
                 sh 'mvn clean package'
          }
        }
        stage('Install tomcat using ansible'){
            steps{
                sshagent(['app-node-ssh']){
                    sh ''' 
                        ansible-playbook -i inventory install_tomcat.yml
                     '''
                }
            }
        }
        stage("deploy the project on tomcat"){
            steps{
                sshagent(['app-node-ssh']){
                    sh ''' 
                        scp target/addressbook.war ec2-user@54.165.139.224:/opt/tomcat/webapps/
                     '''
                }
            }
        }
    }
}
