node {

    environment {
        GIT_EMAIL_CREDS = credentials('github-email')
        GIT_EMAIL = "${GIT_EMAIL_CREDS_USR}"
    }
    
    stage('Clone Repository') {
        checkout scm
    }

    stage('Update GitOps Repo') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email ${GIT_EMAIL}"
                    sh "git config user.name ${GIT_USERNAME}"

                    sh "cat ./${MANIFEST}/${MANIFEST}.yaml"
                    sh "sed -i 's+${DOCKERIMAGE}.*+${DOCKERIMAGE}:${DOCKERTAG}+g' ${MANIFEST}.yaml"
                    sh "cat ./${MANIFEST}/${MANIFEST}.yaml"
                    sh "git add ."
                    sh "git commit -m '${MANIFEST}.yaml Update by Jenkins # ${DOCKERTAG}'"
                    sh "git push https://${GIT_PASSWORD}@github.com/SeSac-Cloud-Final-3/GitOps_Repo.git HEAD:main"
                }
            }
        }
    }
}