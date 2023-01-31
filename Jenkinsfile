
pipeline {
    environment{
    registry="nefzaouiachref/ach"
    registryCredential='83ffc7a1-192e-4250-8139-f45ffdc2bf97'
    dokerImage="devops"
}
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
        stage("docker build") {
                       steps{
                         script {
                            dockerImage = docker.build registry +":$BUILD_NUMBER"
                       }
                 }
       }
        
        stage("docker push") {
              steps{
                 script {
                 withDockerRegistry(credentialsId: registryCredential) {
                  dockerImage.push()
                  }
                }
           }
        }
        stage('Cleaning up') {
             steps{
             sh "docker rmi $registry:$BUILD_NUMBER"
        }
    }
    stage('running containers'){
        steps{
            sh 'docker-compose up -d'
        }
    }
        
    }    

}
