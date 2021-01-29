
pipeline {
    agent any
    environment{
      def mvnHome = tool name: 'maven3', type: 'maven'
      def mvnCMD = "${mvnHome}/bin/mvn"
      def tfHome = tool name: 'Ansible'
      def Version = '1.0'
    }

    stages {
        stage('Git Clone'){
            steps{
              git 'https://github.com/ajibjawwad/java-hello-world-webapp.git'
            }
        }

        stage('Build Package'){
            steps{
              sh  "${mvnCMD} clean package"
            }
        }

        stage('Docker Build'){
            steps{
              sh 'docker build -t jebro/java-hello-world:${Version} .'
            }
        }

        stage('Docker Push'){
            steps{
              withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerpwd')]) {
                sh "docker login -u jebro -p ${dockerpwd}"
              }
              sh 'docker push jebro/java-hello-world:${Version}'
            }
        }

        stage('Ansible Check'){
            steps{
              script{
                env.PATH = "${tfHome}:${env.PATH}"
                sh 'ansible --version'
              }

            }
        }

        stage('Deploy Apps'){
            steps{
              sh 'ansible-playbook ansible-deploy.yml'
            }
        }
    }
}
