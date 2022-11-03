def ARTIFACTORY_ID = 'Artifactory'
def BOT_ID = 'dpt-admin-bot'
pipeline {
    agent {
        kubernetes {
            defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
            yaml '''
              apiVersion: v1
              kind: Pod
              metadata:
                labels:
                  jenkins/kube-default: true
                  app: jenkins
                  component: agent
              spec:
                imagePullSecrets:
                - name: artif1
                containers:
                - name: maven
                  image: artifactory.bankdata.eficode.io/all-docker-centrals/library/maven:latest
                  command: ["sleep", "inf"]
                - name: jnlp
                  image: artifactory.bankdata.eficode.io/all-docker-centrals/jenkins/jnlp-slave:latest
                  '''
        }
    }

    environment {
        ARTIFACTORY_ID = 'Artifactory'
        BOT_ID = 'dpt-admin-bot'
       
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn --version'

            }
        }

        stage('Setup') {
            steps {
                script{
                    container('maven') {
                        sh('ls')
                    

                    }
                }
                }
            }    

    }


}