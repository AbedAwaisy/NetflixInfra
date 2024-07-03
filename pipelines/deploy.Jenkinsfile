pipeline {
    agent {
        label 'general'
    }
    parameters {
        string(name: 'SERVICE_NAME', defaultValue: '', description: '')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: '')
    }

    stages {
        stage('Deploy') {
            steps {
                /*

                Now your turn! implement the pipeline steps ...

                - `cd` into the directory corresponding to the SERVICE_NAME variable.
                - Change the YAML manifests according to the new $IMAGE_FULL_NAME_PARAM parameter.
                  You can do so using `yq` or `sed` command, by a simple Python script, or any other method.
                - Commit the changes, push them to GitHub.
                   * Setting global Git user.name and user.email in 'Manage Jenkins > System' is recommended.
                   * Setting Shell executable to `/bin/bash` in 'Manage Jenkins > System' is recommended.
                */
            }
        }
    }
}