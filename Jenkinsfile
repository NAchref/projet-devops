
pipeline {
    agent any
    stages {
        stage('Cloning project in local VM') {
            steps {
                // clone from git and test trigger
                git branch: 'master', credentialsId: 'a5a1dcc9-8c03-48fe-949b-ca708f5181a4', url: 'https://github.com/NAchref/projet-devops.git'
                echo "-------------------Clone Stage Done ------------------------------- "
            }
        }
        stage("clean project "){
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true clean"
                }
            }
        }
        stage("compile project ") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true compile"
                }
            }
        }
        stage("test project ") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true test"
                }
            }
        }
        stage("build project") {
            steps {
                script {
                    sh "mvn -Dmaven.test.failure.ignore=true clean package"
                }
            }
        }
        stage('deploy project to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'tpAchatProject', classifier: '', file: 'target/tpAchatProject-1.0.jar', type: 'jar']], credentialsId: '128156d6-f04d-4b21-acae-d6794b50ce9f', groupId: 'com.esprit.examen', nexusUrl: '192.168.56.14:8081/repository/maven-releases/', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexusdeploymentrepo', version: '1.0'
            }
        }
        
    }    

}
