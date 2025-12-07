pipeline {
    agent any

    tools {
        maven 'M3'
    }

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: 'github-token',
                    url: 'https://github.com/doniamaaz/studentManagment.git',
                    branch: 'main'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

      /*  stage('Build Docker Image') {
            steps {
                sh 'docker build -t doniaamaazoun/student-management:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push doniaamaazoun/student-management:latest
                    '''
                }
            }
        }*/
        stage('Test with sonarqube') {
            environment {
                SONAR_TOKEN = credentials('jenkins-sonar')
            }
            steps {
                withSonarQubeEnv(installationName: 'sql1') {
                    sh 'echo $SONAR_TOKEN'
                    sh 'mvn sonar:sonar'
                    sh 'mvn clean verify sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
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
