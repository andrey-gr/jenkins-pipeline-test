pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'FIrst stage'
      }
    }
    stage('Proceed to stage?') {
      steps {
        input 'Should we deploy to stage?'
      }
    }
      stage('Tag & Release') {
        steps {
            script {
              def dateStr = new Date().format('yyyy-MM-dd')
              versionTag = "autohub-${dateStr}-#${env.BUILD_ID}"
            }

          sh 'git config user.email "jenkins@jenkins.fino.tech"'
          sh 'git config user.name "Jenkins Pipeline"'
          sh "git tag -a $versionTag -m 'Version deploy'"
          withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GITHUB_TOKEN')]) {
              echo "$env.GITHUB_TOKEN"
              sh "git push --tags"
              sh "github-release release -u resolvit-lv -r longo --tag '${versionTag}' --name '$versionTag' --description \"\nBuild log:\n${env.BUILD_URL}\" --draft --pre-release"
          }
        }
      }
  }
}