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
	      

            }
        }
	stage('Update GIT') {
  steps {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([usernamePassword(credentialsId: 'gitcredentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
            def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
            sh "git config user.email rvishwaja@gmail.com"
            sh "git config user.name VishwajaMarreddy"
            sh "git add ."
            sh "git commit -m 'Triggered Build: ${env.BUILD_NUMBER}'"
            sh "git push https://${GIT_USERNAME}:${encodedPassword}@github.com/${GIT_USERNAME}/project.git"
        }
      }
    }
  }
}
	stage('nexus'){
	  steps{
		sh 'mvn deploy'
		}
	}
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
}
}
