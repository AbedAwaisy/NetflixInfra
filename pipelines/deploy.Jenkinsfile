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
                    // Change to the directory containing the deployment YAML
                    dir("k8s/NetflixFrontend") {
                        // Update the image in the deployment YAML
                        sh '''
                            yq e '(.spec.template.spec.containers[] | select(.name == "netflix-frontend").image) = "'"${IMAGE_FULL_NAME_PARAM}"'"' -i deployment-netflix-frontend.yaml

                            # Verify the changes
                            cat deployment-netflix-frontend.yaml
                        '''

                        // Commit and push the changes
                        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                            sh '''
                                git config --global user.name "Abed Awaisy"
                                git config --global user.email "abed.awaisy.a@gmail.com"
                                git add deployment-netflix-frontend.yaml
                                git commit -m "Update image to ${IMAGE_FULL_NAME_PARAM}"
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/AbedAwaisy/NetflixInfra.git HEAD:main
                            '''
                        }
                    }
                }
            }
        }
    }
}
