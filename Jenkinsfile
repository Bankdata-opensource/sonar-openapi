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
                - name: maven
                  image: artifactory.bankdata.eficode.io/all-docker-centrals/library/maven:latest
                  command: ["sleep", "inf"]
                - name: jnlp
                  image: artifactory.bankdata.eficode.io/all-docker-centrals/jenkins/jnlp-slave:latest
                  '''
        }
    }

    environment {
      /*  ARTIFACTORY_CREDS = with.credentials('NexusArtifactoryLogin')
        ARTIFACTORY_USER = "$NEXUS_CREDS_USR"
        ARTIFACTORY_PASSWORD = "$NEXUS_CREDS_PSW"
       */
    }

    stages {
        
        stage('Setup') {
            steps {
                script{
                    container('maven') {
                        withCredentials([usernamePassword(credentialsId: "${BOT_ID}", usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                                echo ARTIFACTORY_USER
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