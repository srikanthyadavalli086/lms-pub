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
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Release') {
            steps {
                script {
                    echo "Releasing.."
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"
                    sh "zip dist-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:Admin --upload-file dist-${packageJSONVersion}.zip http://52.207.91.68:8081/repository/lms/"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying.."
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"
                    sh "curl -u admin:Admin -X GET \'http://52.207.91.68:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r dist/* /var/www/html"
                }
            }
        }
    }
}