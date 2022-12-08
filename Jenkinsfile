def ARTIFACTORY_ID = 'Artifactory'
    def BOT_ID = 'son-sonaropenapi-artifactory-bot'
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
                - name: mavenbak
                  image: artifactory.bankdata.eficode.io/all-docker-centrals/library/maven:latest
                  command: ["sleep", "inf"]
                - name: jnlp
                  image: artifactory.bankdata.eficode.io/all-docker-release/jenkins/inbound-agent:latest
                - name: maven
                  image: artifactory.bankdata.eficode.io/docker-hub//library/maven:3.8.6-openjdk-8
                  command: ["sleep", "inf"]
                  '''
        }
    } 
//køre 
    stages {
        stage('Setup') {
            steps {
                script {
                    container('maven') {
                        withCredentials([usernamePassword(credentialsId: "${BOT_ID}", usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                                echo ARTIFACTORY_USER
                                sh("java -version")
                                // or inside double quotes for string interpolation
                                echo "username is $ARTIFACTORY_USER"
                                sh('mvn -s settings.xml package')
                                
                        }
                    }
                }
            }
        }
    }
}
