pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    environment {
        ARTIFACTORY_URL = 'http://35.154.215.87:8081/artifactory'
        ARTIFACTORY_ID = 'Jfrog'
        POM_PATH = 'pom.xml'
        REPO_RELEASE = 'example-repo-local'
        REPO_SNAPSHOT = 'New-maven-snapshots'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/techworldwithmurali/web-application-old.git'
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    def server = Artifactory.server(ARTIFACTORY_ID)
                    def rtMaven = Artifactory.newMavenBuild()
                    def buildInfo = Artifactory.newBuildInfo()

                    rtMaven.tool = 'Maven3'
                    rtMaven.resolver server: server, releaseRepo: REPO_RELEASE, snapshotRepo: REPO_SNAPSHOT
                    rtMaven.deployer server: server, releaseRepo: REPO_RELEASE, snapshotRepo: REPO_SNAPSHOT
                    rtMaven.deployer.deployArtifacts = true

                    rtMaven.run pom: POM_PATH, goals: 'clean install', buildInfo: buildInfo
                    server.publishBuildInfo buildInfo
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
