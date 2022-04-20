node {
    
    stage('Clone Repository') {
        checkout scm
    }

    stage('Update GitOps Repo') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email dev_210@naver.com"
                    sh "git config user.name onzero98"

                    sh "cat ${MANIFEST}.yaml"
                    sh "sed -i 's+${DOCKERIMAGE}.*+${DOCKERIMAGE}:${DOCKERTAG}+g' ${MANIFEST}.yaml"
                    sh "cat ${MANIFEST}.yaml"
                    sh "git add ."
                    sh "git commit -m 'GitOps Repo Update by Jenkins num:${env.BUILD_NUMBER}'"
                    sh "git push --force https://${GIT_PASSWORD}@github.com/SeSac-Cloud-Final-3/GitOps_Repo.git HEAD:main"
                }
            }
        }
    }
}