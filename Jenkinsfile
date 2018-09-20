pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '18.216.160.250', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.15.81.206', description: 'Production Server')
         string(name: 'aws-key', defaultValue: 'D:/project/Jenkins/AWS/tomcat-demo.pem', description: 'AWS key full path')
    }

    triggers {
        pollSCM('* * * * *')
    }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "scp -i ${params.aws-key} **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i ${params.aws-key} **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
   
}