pipeline {
    agent {
        label 'general'
    }
    parameters {
        string(name: 'SERVICE_NAME', defaultValue: '', description: 'The name of the service to deploy')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: 'The full Docker image name to deploy')
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    // Change to the directory containing the deployment YAML
                    dir("k8s/NetflixFrontend") {
                        // Update the image in the deployment YAML using sed
                        sh """
                            sed -i 's|image: .*|image: ${IMAGE_FULL_NAME_PARAM}|' deployment-netflix-frontend.yaml

                            # Verify the changes
                            cat deployment-netflix-frontend.yaml
                        """

                        // Commit and push the changes
                        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                            sh """
                                git config --global user.name "Abed Awaisy"
                                git config --global user.email "abed.awaisy.a@gmail.com"
                                git add deployment-netflix-frontend.yaml
                                git commit -m 'Update image to ${IMAGE_FULL_NAME_PARAM}'
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/AbedAwaisy/NetflixInfra.git HEAD:main
                            """
                        }
                    }
                }
            }
        }
    }
}
