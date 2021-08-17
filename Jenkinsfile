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
	sh """
	mvn sonar:sonar -Dsonar.host.url=http://${env.'ifconfig'.execute().text.split('/')[2].split(':')[0]}:9000 -Dsonar.login=2fecc47f1f0955e88f4c509042373a99e7779408
	"""
        }
	}
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
