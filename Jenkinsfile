pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    environment {
        ARTIFACTORY_URL = 'http://35.154.215.87:8081/artifactory'
        ARTIFACTORY_ID = 'Jfrog'
        POM_PATH = 'pom.xml'
        VIRTUAL_REPO = 'libs-release'
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
                    rtMaven.opts = '-U'
                    rtMaven.resolver server: server, releaseRepo: VIRTUAL_REPO, snapshotRepo: VIRTUAL_REPO
                    rtMaven.deployer server: server, releaseRepo: 'example-repo-local1', snapshotRepo: 'New-maven-snapshots'
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
