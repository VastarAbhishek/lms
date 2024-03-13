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

        }
}

