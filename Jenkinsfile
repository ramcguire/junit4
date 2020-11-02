pipeline {
    agent any
    environment {
      ARTIFACTORY_CREDS = credentials('Artifactory')
    }
    stages {
        stage ('Get working directory') {
          steps {
            sh 'pwd'
          }
        }
        stage ('Compile') {
            steps {
                sh 'mvn -s /usr/share/java/maven-3/conf/settings.xml compile'
            }
        }

        stage ('Test') {
          steps {
              sh 'mvn -s /usr/share/java/maven-3/conf/settings.xml test'
          }
        }

        stage ('Package') {
          steps {
              sh 'mvn -s /usr/share/java/maven-3/conf/settings.xml package'
          }
        }

        stage ('Deploy') {
            steps { 
              if (env.BRANCH_NAME == "main") {
                sh 'curl -u $ARTIFACTORY_CREDS -X PUT "http://artifactory:8081/artifactory/libs-snapshot/junit4-pipeline/junit4-pipeline.jar" -T ./target/junit4-pipeline.jar'
              } else {
                echo "Skipping deploy for $env.BRANCH_NAME"
              }
            }
        }
    }
}