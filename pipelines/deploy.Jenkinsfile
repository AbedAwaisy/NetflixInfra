pipeline {
    agent {
        label 'general'
    }
    parameters {
        string(name: 'SERVICE_NAME', defaultValue: '', description: 'The name of the service to deploy')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: 'The full Docker image name to deploy')
    }

    stages {
        stage('Install yq') {
            steps {
                sh '''
                    if ! command -v yq &> /dev/null
                    then
                        echo "yq could not be found, installing..."
                        mkdir -p ${HOME}/bin
                        curl -L https://github.com/mikefarah/yq/releases/download/v4.6.3/yq_linux_amd64 -o ${HOME}/bin/yq
                        chmod +x ${HOME}/bin/yq
                        export PATH=${HOME}/bin:$PATH
                    else
                        echo "yq is already installed"
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Ensure yq is available in the PATH
                    env.PATH = "${env.HOME}/bin:${env.PATH}"
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
