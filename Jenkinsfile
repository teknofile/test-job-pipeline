pipeline {
   agent any

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
                IMAGENAME="teknofile/testimage"
                TAG=$(date "+%H%M%S%d%m%Y")
                docker build -t $IMAGENAME:$TAG .
                docker push $IMAGENAME:$TAG
                # echo "$IMAGENAME:$TAG ${WORKSPACE}/Dockerfile " > anchore_images
                echo "$IMAGENAME:$TAG" > anchore_images
            '''
          }
      }
      stage('Scan it') {
        steps {
            anchore engineRetries: '3000', name: 'anchore_images'
        }
      }
   }
}

