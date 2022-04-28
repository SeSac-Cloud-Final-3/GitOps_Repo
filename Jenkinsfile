node {
    stage('Clone Repository') {
        checkout scm
    }

    stage('Update GitOps Repo') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([
                    usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME'),
                ]) {
                    sh "cat ./${MANIFEST}/${MANIFEST}.yaml"
                    sh "sed -i 's+${DOCKERIMAGE}.*+${DOCKERIMAGE}:${DOCKERTAG}+g' ./${MANIFEST}/${MANIFEST}.yaml"
                    sh "cat ./${MANIFEST}/${MANIFEST}.yaml"
                    sh "git add ."
                    sh "git commit -m '${MANIFEST}.yaml Update by Jenkins # ${DOCKERTAG}'"
                    sh "git push https://${GIT_PASSWORD}@github.com/SeSac-Cloud-Final-3/GitOps_Repo.git HEAD:main"
                }
            }
        }
    }
}