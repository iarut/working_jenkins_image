pipeline {
 agent any

 environment {
  DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
 }

       triggers {
                      pollSCM "*/3 * * * *"
                      cron "13 7 * * * "
//                       GenericTrigger(
//                                   causeString: "Triggered from Webhook",
//                                   token: "unique-token-to-start-the-current-pipeline"
//                               )
                  }


 stages {
  stage('Checkout Github') {
   steps {
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/sushant032/jenkins-build.git'
   }
  }

  stage('Build Code') {
   steps {
    sh './gradlew build'
   }
  }

  stage('Login') {
   steps {
    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
   }
  }

  stage('Build docker image') {
   steps {
    sh 'docker build --tag=iarut/jenkins-build:latest .'
    sh 'docker push iarut/jenkins-build:latest'
   }
  }
 }

 post {
  always {
   sh 'docker logout'
  }
 }
}