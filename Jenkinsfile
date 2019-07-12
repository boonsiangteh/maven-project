pipeline {
    agent any

    parameters {
        string(name: 'tomcat_staging', defaultValue: '3.1.202.148', description: 'ip address for tomcat staging')
        string(name: 'tomcat_production', defaultValue: '18.139.3.155', description: 'ip address for tomcat production')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
            post {
                success {
                    echo "Now archiving ..."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging'){
                    steps {
                        sh "scp -i tomcat-staging.pem ec2-user@${params.tomcat_staging}:/var/lib/webapps"
                    }
                    post {
                        success {
                            echo "Deployed to Staging"
                        }
                    }
                }
                stage('Deploy to Production'){
                    steps {
                        sh "scp -i tomcat-production.pem ec2-user@${params.tomcat_production}:/var/lib/webapps"
                    }
                    post {
                        success {
                            echo "Deployed to Production"
                        }
                    }
                }
            }
        }
    }
}
