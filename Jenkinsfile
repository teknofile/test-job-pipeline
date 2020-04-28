pipeline {
   agent {
    label 'X86-64-MULTI'
  }

  environment {
    IMAGENAME="teknofile/testimage"
    TAG="testing"
  }
   stages {
      stage('Create Dockerfile') {
         steps {
            sh '''
                echo 'FROM python' > ${WORKSPACE}/Dockerfile
            '''
         }
      }
      stage('Build/Tag Container') {
          steps {
              sh '''
                docker build -t $IMAGENAME:$TAG .
                docker push $IMAGENAME:$TAG
            '''
          }
      }
      stage('Scan it') {
        steps {
            sh 'curl -s https://nexus.copperdale.teknofile.net/repository/teknofile-utils/teknofile/ci/utils/inline_scan-v0.6.0 | bash -s -- -t 1800 -p docker.copperdale.teknofile.net/${IMAGENAME}:${TAG} -r'
        }
      }
   }
}

