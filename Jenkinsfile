pipeline {
  agent any

  environment {
    IMAGE = "shivamsoni1995/flask-app-argocd"
    TAG = "v${BUILD_NUMBER}"
    BRANCH = "main"
    GIT_REPO = "https://github.com/ShivamSoni1995/Flask-App-ArgoCD.git"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: "${BRANCH}", credentialsId: 'github-creds', url: "${GIT_REPO}"
      }
    }

    stage('Build & Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {         
          sh "docker build -t ${IMAGE}:${TAG} ."
          sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
          sh "docker push ${IMAGE}:${TAG}"
        }
      }
    }

    stage('Update Image in deployment.yml') {
      steps {
        sh "yq e '.spec.template.spec.containers[0].image = \"${IMAGE}:${TAG}\"' -i deployment.yml"
      }
    }

    stage('Commit & Push updated deployment.yml') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'github-creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
          sh """
            git config user.name "jenkins-bot"
            git config user.email "jenkins-bot@example.com"
            git add deployment.yml
            git commit -m "Update image tag to ${TAG} [ci skip]" || echo 'No changes'
            git push https://${GIT_USER}:${GIT_PASS}@github.com/ShivamSoni1995/Flask-App-ArgoCD.git ${BRANCH}
          """
          
        }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}

