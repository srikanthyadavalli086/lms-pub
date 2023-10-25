pipeline {
    agent {
        label 'docker2'
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
                    sh "curl -v -u admin:Admin --upload-file webapp/dist-${packageJSONVersion}.zip http://54.197.64.109:8081/repository/lms/"     
            }
            }
        }

        stage('Deploy LMS App') {
            steps {
                script {
                    echo "Deploying.."       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "curl -u admin:Admin -X GET \'http://54.197.64.109:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
            }
            }
        }
        
}
