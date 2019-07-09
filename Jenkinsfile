pipeline {
    agent any
    tools {
        maven 'localMaven'
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
        stage('deploy to staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage('deploy to production'){
            steps {
                timeout(time: 5, unit:'DAYS'){
                    input message: 'Deploy to Production ?'
                }
                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo 'Code deployed to production'
                }
                failure {
                    echo 'Build failure'
                }
            }
        }
    }
}
