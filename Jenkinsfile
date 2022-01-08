pipeline {
    agent any
    
    environment {
        // This can be nexus3 or nexus2 server
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "localhost:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY_RELEASES = "maven-releases"
        NEXUS_REPOSITORY_SNAPSHOTS = "maven-snapshots"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"
    }
    
    stages {
	stage('Quality check') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=11807b22879950e9cc1a90e05b7b7b5c4cb253d4'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Nexus Repository') {
            steps {
                sh 'mvn clean package deploy:deploy-file -DgroupId=tn.esprit -DartifactId=timesheet-ci -Dversion=6.2 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://localhost:8081/repository/maven-releases/ -Dfile=target/timesheet-ci-1.0.jar'
            }
        }
    }
}
