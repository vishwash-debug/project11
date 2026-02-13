pipeline {
    agent { label "${netli}"}

    environment {
        IMAGE_NAME = "netapp"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('CODE') {
            steps {
                git url:"https://github.com/vishwash-debug/project11", branch:"main"
            }
        }

        stage('Build') {
            steps {
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }

        stage('Deploy') {
            steps {
                    sh """
                      docker stop net1 || true
                      docker rm net1 || true
                      docker run -d --name net1 -p 80:80 --restart always ${DOCKER_IMAGE}
                    """
                }
        }
    }

    post {

        success {
            archiveArtifacts artifacts: '*.tar', fingerprint: true

            emailext(
                subject: "SUCCESS: Job '${JOB_NAME} [${BUILD_NUMBER}]'",
                body: """
                    <p>Build Successful</p>
                    <p>Job: ${JOB_NAME}</p>
                    <p>Build Number: ${BUILD_NUMBER}</p>
                    <p>Docker Image: ${DOCKER_IMAGE}</p>
                """,
                mimeType: 'text/html',
                to: 'prazwalvishwash@gmail.com'
            )
        }

        failure {
            emailext(
                subject: "FAILED: Job '${JOB_NAME} [${BUILD_NUMBER}]'",
                body: """
                    <p>Build Failed</p>
                    <p>Job: ${JOB_NAME}</p>
                    <p>Build Number: ${BUILD_NUMBER}</p>
                    <p>Please check Jenkins logs.</p>
                """,
                mimeType: 'text/html',
                to: 'prazwalvishwash@gmail.com'
            )
        }
    }
}
