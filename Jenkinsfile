pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_stag', defaultValue: '54.183.188.6', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.144.101.115', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('package'){
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
                        sh "scp -i /home/subham/.ssh/id_rsa.pub **/target/*.war ec2-user@${params.tomcat_stag}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/subham/.ssh/id_rsa.pub **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}   