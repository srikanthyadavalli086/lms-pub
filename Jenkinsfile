pipeline {
    agent any
    stages {
        stage('Sonarqube Analysis') {
            steps {
                echo "Analysis..."
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://52.207.91.68:9000" -e SONAR_LOGIN="sqp_3a6f4a7e6f38fd55d9dc7b9762648f808eceddf9" -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        stage('Build') {
            steps {
                echo "Building..."
            }
        }
        stage('Test') {
            steps {
                echo "Testing..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying.."
                echo "Success"
            }
        }
    }
}