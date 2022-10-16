pipeline {
  agent {
    label "kube-agent"
  }
  
  environment {
    TAG = sh (script: "date +%y%m%d%H%M", returnStdout: true).trim()
  }
  
  stages {
    stage('Preparation') {
      steps {
        container('webapp-agent') {
          sh "echo App Version = $TAG"
        }
      }
    }
    stage('Update manifest') {
      steps {
        container('webapp-agent') {
          sh """
            git clone https://github.com/btech-training-team/simple-webapp-manifest.git
            cd simple-webapp-manifest
            sed -i "s/webapp:.*/webapp:$TAG/g" deployment.yaml
            """
          script {
            withCredentials([usernamePassword(credentialsId: 'github-cred',
                 usernameVariable: 'username',
                 passwordVariable: 'password')]){
                  sh("""
                  git config --global user.email "example@main.com"
                  git config --global user.name "example"
                  git add . && git commit -m 'update image tag'
                  git push https://$username:$password@github.com/btech-training-team/simple-webapp-manifest.git
                  """)
            }
          }
        }
      }
    }
/*
    stage('Test') {
      steps {
        container('webapp-agent') {
          sh "html5validator html/index.html"
        }
      }
      post {
        success {
           echo "Test Successful"
        }
        failure {
           echo "Test Failed"
        }
      }
    }
    
    stage("Build & Push Image"){
      steps{
        container('kaniko'){
          sh """
          /kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` \
          --verbosity debug --destination docker.io/atwatanmalikm/webapp:$TAG
          """
        }
      }
    }

    stage("Update Manifest"){
      steps{
        container('kaniko'){
          sh """
          /kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` \
          --verbosity debug --destination docker.io/atwatanmalikm/webapp:$TAG
          """
        }
      }
    }
*/
  }
}
