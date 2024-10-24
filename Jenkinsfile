pipeline {
    agent any
    environment {
        ARTIFACTORY_URL = 'http://35.154.215.87:8081/artifactory'
        ARTIFACTORY_ID = 'Artifactory'
        MAVEN_VERSION = 'Maven3'
        POM_PATH = 'pom.xml'
        REPO_RELEASE = 'example-repo-local'
        REPO_SNAPSHOT = 'New-maven-snapshots'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Murali's public GitHub repository
                git branch: 'master', url: 'https://github.com/techworldwithmurali/web-application-old.git'
            }
        }
        stage('Artifactory Configuration') {
            steps {
                script {
                    def server = Artifactory.server(ARTIFACTORY_ID)
                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.resolver server: server, releaseRepo: REPO_RELEASE, snapshotRepo: REPO_SNAPSHOT
                    rtMaven.deployer server: server, releaseRepo: REPO_RELEASE, snapshotRepo: REPO_SNAPSHOT
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Run the Maven build and deploy the artifacts
                    def rtMaven = Artifactory.newMavenBuild()
                    rtMaven.run pom: POM_PATH, goals: 'clean install'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and Artifactory deploy successful!'
        }
        failure {
            echo 'Build or Artifactory deploy failed!'
        }
    }
}
