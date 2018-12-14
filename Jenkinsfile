pipeline {
    agent any
    
    parameters{
        string(name: 'tomcat_dev', defaultValue: '13.59.23.201', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.191.29.222', description: 'Production Server')         
    }
    
    stages{
        stage ('Build') {
            steps {
                bat 'mvn clean package'
                bat "docker build . -t 1953/tomcatwebapp:${env.BUILD_ID}"
                bat "docker image push 1953/tomcatwebapp"
                bat "docker container run -d -p 8090:8080 1953/tomcatwebapp"
            }
            post {
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }    
}