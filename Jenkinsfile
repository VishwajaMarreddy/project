pipeline {
    agent any
    
    environment {
    //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
    //IMAGE = readMavenPom().getArtifactId()
    //version = readMavenPom().getVersion()
   def version = sh script: 'mvn help:evaluate -Dexpression=project.version -q -DforceStdout', returnStdout: true
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
        mvn sonar:sonar -Dsonar.host.url=http://54.160.226.171:9000 -Dsonar.login=2fecc47f1f0955e88f4c509042373a99e7779408
        """
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
                sh "wget --user admin --password admin123 http://54.160.226.171:8081/nexus/service/local/repositories/releases/content/com/web/cal/WebAppCal/${version}/WebAppCal-${version}.war"
		sh "sudo cp WebAppCal-${version}.war  /home/centos/apache-tomcat-7.0.94/webapps/"
            }
        }
}
}
