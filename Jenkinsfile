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
	mvn sonar:sonar -Dsonar.host.url=http://54.227.4.142:9000 -Dsonar.login=2fecc47f1f0955e88f4c509042373a99e7779408
	"""
        }
	}
	      stage('Change pom file') {
            steps {
	      sh "git checkout master"
              sh "mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion}"
	      sh "mvn versions:commit"
	      sh "git add ."
	      sh "git commit -m 'updated version'"

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
