pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.216.122.0', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.188.187.2', description: 'Production Server')
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
                        sh "scp -i c:s/BO/LearningMaterial/Udemy/Mastering Jenkins/TomcatDemo.pem /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i "c:/BO/LearningMaterial/Udemy/Mastering Jenkins/TomcatDemo.pem"  **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}