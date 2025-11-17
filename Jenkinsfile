pipeline {
    agent any

    stages {

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
