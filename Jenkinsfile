pipeline {
  agent any

  environment{
    registry = "jtrevinodev/guestbook"
    registryCredential = 'docker-hub-credential'
    app = ''

    image_tag = "${env.BRANCH_NAME}-${env.GIT_COMMIT}-${env.BUILD_NUMBER}"
  }

  stages {

    stage('Clone repository') {

      steps { 
        script{
          checkout scm
        }
      }

    }

    

    stage('Build docker image') {

      steps {

        script{
          echo "Building docker container"
          app = docker.build("jtrevinodev/guestbook", "src/php-redis")
        }

      }

    }

    stage('Push container image to registry'){
      
      steps{

        script{
          docker.withRegistry('', registryCredential ) {
            app.push(image_tag)

          }
        }

        
        
      }
       
    }
    

    stage('Deploy to Kubernetes') {
      steps{
        script{
          sh('echo "Deploying to production environment"')
          
          sh 'echo "Clonning deployment repository"'

          dir("deploy") {
            git credentialsId: 'github-key', url: 'git@github.com:jtrevinodev/guestbook-devops-deploy.git'

            def frontend_df = "base/resources/frontend-deployment.yaml"
            def frontend_deployment = readFile frontend_df
            frontend_deployment = frontend_deployment.replaceAll("image:.*", "image: jtrevinodev/guestbook:${image_tag}")
            writeFile file: frontend_df, text: frontend_deployment
            sh("cat ${frontend_df}")

            sh 'echo "Pushing deployment config to deployment repository"'
            
            
            withCredentials([string(credentialsId: 'github-jenkins-ak', variable: 'TOKEN')]) {
            
              sh 'git config --global user.email "jtrevino.dev@gmail.com"'
              sh 'git config --global user.name "Jenkins pipeline"'

              sh "git add ${frontend_df}"
              sh 'git commit -m "image tag updated: ${image_tag}"'
              
              sh 'git remote set-url origin https://$TOKEN@github.com/jtrevinodev/guestbook-devops-deploy.git'

              sh 'git push origin master'
            }

          }
        }
        
      }
      
      

    }

  }
  post {
    success {
      mail to:"jtrevino.dev@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build succeded."
    }
    failure {
      mail to:"jtrevino.dev@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
    }
  }

}

