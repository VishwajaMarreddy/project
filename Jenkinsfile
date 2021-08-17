pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
	stage('sonar') {
           steps {
		sh " echo '${env.BUILD_URL.split('/')[2].split(':')[0]}' "
        }
	}
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
