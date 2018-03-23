pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9090', description: 'Production Server')
    } 
    
    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "cp /c/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war /c/DevTools/apache-tomcat-8.5.29-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp /c/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war /c/DevTools/apache-tomcat-8.5.29-prod/webapps"
                    }
                }
            }
        }
    }
}