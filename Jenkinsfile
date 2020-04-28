pipeline {
   agent {
    label 'X86-64-MULTI'
  }

  environment {
    IMAGENAME="teknofile/testimage"
    TAG="testing"
    LOCAL_DOCKER_PROXY="docker.copperdale.teknofile.net/"
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
            sh 'curl -s https://nexus.copperdale.teknofile.net/repository/teknofile-utils/teknofile/ci/utils/tkf-inline-scan-v0.6.0-1.sh | bash -s -- -t 1800 -p ${LOCAL_DOCKER_PROXY}${IMAGENAME}:${TAG} -r'
        }
      }
   }
}

