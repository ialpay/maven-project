pipeline {
    agent any
 
    tools {
        maven 'jenkinsMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.88.41.112', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.210.192.34', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "echo y | scp -i /home/jenkins/Shabanmanus2.pem **/target/*.war ec2-user@${params.tomcat_dev}:/home/ec2-user"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "echo y | scp -i /home/jenkins/Shabanmanus2.pem **/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user"
                    }
                }
            }
        }
    }

}


