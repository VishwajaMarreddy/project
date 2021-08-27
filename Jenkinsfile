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
	stage('Change pom file') {
            steps {
	      sh "git checkout master"
              sh "mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion}"
	      sh "mvn versions:commit"
	      sh "git add ."
	      sh 'git commit -m "added pom"'
	      sh "git config --global push.default simple"
	      sh "git push --all https://github.com/VishwajaMarreddy/project.git"

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
