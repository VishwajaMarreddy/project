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
        mvn sonar:sonar -Dsonar.host.url=http://52.90.155.192:9000 -Dsonar.login=2fecc47f1f0955e88f4c509042373a99e7779408
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
	stage('Nexus')
	{
	   steps{
		sh "mvn deploy"
	}
	}
        stage('Deploy') {
            steps {
                sh "wget --user admin --password admin123 http://52.90.155.192:8081/nexus/service/local/repositories/releases/content/com/web/cal/WebAppCal/${DnewVersion}/WebAppCal-${DnewVersion}.war"
		sh "sudo cp WebAppCal-${DnewVersion}.war  /home/centos/apache-tomcat-7.0.94/webapps/"
            }
        }
}
}
