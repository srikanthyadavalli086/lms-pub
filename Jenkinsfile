pipeline {
    agent any

    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'Analysising code....'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://3.83.98.175:9000" -e SONAR_LOGIN="sqp_05514a1a07ade4b54aa305a800348f343104c8fd" -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        
        stage('Build LMS') {
            steps {
                echo 'Building app....'
                sh 'cd webapp && npm install && npm run build'
            }
        }

        stage('Release LMS app') {
            steps {
                script {
                    echo "Releasing.."       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "zip webapp/dist-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:Admin --upload-file webapp/dist-${packageJSONVersion}.zip http://3.83.98.175:8081/repository/lms/"     
            }
            }
        }

        stage('Deploy LMS') {
            steps {
                script {
                    echo "Deploying.."       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "curl -u admin:Admin -X GET \'http://3.83.98.175:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
            }
            }
        }
        
    }
}
