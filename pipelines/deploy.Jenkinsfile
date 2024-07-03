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
                    // Change to the directory corresponding to the service name
                    dir("${params.SERVICE_NAME}") {
                        // Update the image in the deployment YAML
                        sh '''
                            yq eval '.spec.template.spec.containers[0].image = env.IMAGE_FULL_NAME_PARAM' -i deployment-netflix-frontend.yaml

                            # Verify the changes
                            cat deployment-netflix-frontend.yaml

                            # Add and commit the changes
                            git config --global user.name "Abed Awaisy"
                            git config --global user.email "abed.awaisy.a@gmail.com"
                            git add deployment-netflix-frontend.yaml
                            git commit -m "Update image to ${IMAGE_FULL_NAME_PARAM}"
                            git push origin HEAD:main
                        '''
                    }
                }
            }
        }
    }
}
