pipeline {
    agent any 
    stages {
        stage('SonarAnalysis') { 
            steps {
                sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://172.206.18.185:9000" -e SONAR_LOGIN="sqp_a68c1aae2a6a011f2a5ca2e1ffcfef8d603f74c0"  -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms' 
            }
        }
	stage('BuildTheCode') {
            steps {
                sh 'cd webapp && npm install && npm run build'
            }
        }

	stage('Release LMS') {
            steps {
                script {
                    echo "Publish LMS Artifacts"
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/dist-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:lms12345 --upload-file webapp/dist-${packageJSONVersion}.zip http://172.206.18.185:8081/repository/lms/"
            }
            }
        }

	 stage('Deploy LMS') {
            steps {
                script {
                    echo "Deploy LMS"
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"
                    sh "curl -u admin:lms12345 -X GET \'http://172.206.18.185:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
            }
            }
        }



        }
}

