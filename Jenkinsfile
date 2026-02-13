pipeline {
    agent { label "${LABEL_NAME}" }
    environment { 
        IMAGE_NAME = "simpleapp123"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
        
    }
    stages {
        stage ( 'CODE' ) {
            steps {
                git url:"https://github.com/vishwash-debug/project11.git" , branch: "main"                 
            }
        }
        stage ( 'build' ) {
             steps {
                 sh "docker build -t ${DOCKER_IMAGE} ."
             }
        }   
        
         stage ('deploy') {
               steps {
                   sh "docker stop c1 || true"
                   sh "docker rm c1 || true"
                   sh "docker run -d --name c1 -p 80:80 ${DOCKER_IMAGE} sleep infinity"
                   
               }
    }
}
    post {
        success {
            emailext(
            body: '''THIS MAIL IS REGARDING THE successful BUILD.
FOR THE REFERENCE CHECK COSNSOLE OUTPUT OF ${BUILD_NUMBER}''', 
    subject: 'Congratulationsss Build successful ${BUILD_NAME}', 
    to: 'prazwalvishwash@gmail.com'
            )
        }
        failure {
            emailext(
            body: '''THIS MAIL IS REGARDING THE FAILED BUILD.
FOR THE REFERENCE CHECK COSNSOLE OUTPUT OF ${BUILD_NUMBER}''', 
    subject: 'WARNING!!!!! Build Failed ${BUILD_NAME}', 
    to: 'prazwalvishwash@gmail.com'
            )
        }

                }
            
        
}
