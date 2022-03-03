def CONNECT = 'https://testing.coverity.synopsys.com'
def coverityInstanceUrl = 'https://testing.coverity.synopsys.com'
def PROJECT = 'proftpd-Demo'
def STREAM = "proftpd-Demo-Jenkins"

pipeline {
  
  agent any
  
  stages {
    stage("First Build Step") {
      steps {
        echo 'building the application...'
      }
    }
   stage("Next Build Step") {
     steps {
       echo 'testing the application...'
     }
   }
   stage("Coverity Inject") {
     steps {
       withCoverityEnvironment(coverityInstanceUrl: 'http://192.168.56.103:8080', projectName: 'Python-Demo', streamName: 'Python-Demo-Jenkins', viewName: 'Outstanding Issues') {
    // some block
      }
     }
   }
   stage('Coverity-build') {
        steps {
            script {
                sh "/Applications/cov-analysis-macosx-2021.9.0/bin/cov-build --dir idir /Users/dylanm/Build-Tools/apache-maven-3.8.3/bin/mvn -B clean package -DskipTests"
            }
        }
    }
   stage('Coverity-analyze') {
        steps {
            script {
              sh "/Applications/cov-analysis-macosx-2021.9.0/bin/cov-analyze --dir idir --strip-path $WORKSPACE --webapp-security"
            }
        }
    }
   stage('Coverity-commit') {
        steps {
            script {
              sh "/Applications/cov-analysis-macosx-2021.9.0/bin/cov-commit-defects --dir idir --url http://192.168.56.103:8080 --stream Python-Demo-Jenkins --scm git --user admin --password SIGpass8!"
            }
        }
    }
   stage("BlackDuck Detect") {
      steps {
       script {
         synopsys_detect '--detect.tools=DETECTOR --detect.project.name=SPM-${JOB_NAME} --detect.project.version.name=${GIT_BRANCH} --detect.maven.path=/Users/dylanm/Build-Tools/apache-maven-3.8.3/bin/mvn'
         sh 'blackduck-c-cpp --config audour-config.yaml'
      }
     }
   }
  }
 }
