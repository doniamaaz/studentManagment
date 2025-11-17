pipeline {
    agent any

    tools {
        maven 'M3'   // Le nom configuré dans Manage Jenkins → Tools → Maven
    }

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: 'github-token',
                    url: 'https://github.com/doniamaaz/studentManagment.git',
                    branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "❌ Build Failed: ${env.JOB_NAME}",
                body: "Le build Jenkins a échoué.\nJob: ${env.JOB_NAME}\nBuild: ${env.BUILD_NUMBER}",
                to: "doniamaazoun5@gmail.com"
            )
        }
    }
}
