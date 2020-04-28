pipeline {
   agent {
    label 'X86-64-MULTI'
  }

  environment {
    IMAGENAME="teknofile/testimage"
    TAG=$(date '+%H%M%S%d%m%Y')
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
                # echo "$IMAGENAME:$TAG ${WORKSPACE}/Dockerfile " > anchore_images
                echo "$IMAGENAME:$TAG" > anchore_images
            '''
          }
      }
      stage('Scan it') {
        steps {
            sh 'curl -s https://nexus.copperdale.teknofile.net/repository/teknofile-utils/teknofile/ci/utils/inline_scan-v0.6.0 | bash -s -- -d Dockerfile docker.copperdale.teknofile.net/${IMAGENAME}:${TAG}'
        }
      }
   }
}

