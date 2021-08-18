pipeline {
    agent any
    tools {
        maven 'Maven 3.6.0'
    }

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
	mvn sonar:sonar -Dsonar.host.url=http://54.86.34.85:9000 -Dsonar.login=2fecc47f1f0955e88f4c509042373a99e7779408
	"""
        }
	}
	      stage('Change pom file') {
            steps {
                sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\${parsedVersion.majorVersion}.\\${parsedVersion.minorVersion}.\\${parsedVersion.nextIncrementalVersion}'
                sh "mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion}"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
