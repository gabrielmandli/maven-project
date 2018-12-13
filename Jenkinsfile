pipeline {
    agent any
    
    parameters{
        string(name: 'tomcat_dev', defaultValue: '13.59.23.201', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.191.29.222', description: 'Production Server')            
    }

    triggers{
        pollSCM('* * * * *')
    }
    
    stages{
        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments') {
            parallel{
                stage ('Deploy to Staging'){
                    steps{
                        bat "pscp -i C:/sshkeys/tomcat-demo.ppk /target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ('Deploy to Production'){
                    steps {
                        bat "pscp -i C:/sshkeys/tomcat-demo.ppk /target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }    
}