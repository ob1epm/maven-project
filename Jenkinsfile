pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.216.122.0', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.188.187.2', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
						bat "pscp -scp -v -i c:/BO/LearningMaterial/Udemy/MasteringJenkins/TomcatDemo.ppk c:\Program Files (x86)\Jenkins\workspace\FullyAutomated\webapp\target\webapp.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {                        
						bat "pscp -scp -v -i c:/BO/LearningMaterial/Udemy/MasteringJenkins/TomcatDemo.ppk c:\Program Files (x86)\Jenkins\workspace\FullyAutomated\webapp\target\webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}