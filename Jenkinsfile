pipeline {

  agent any
  /*agent {

    kubernetes {

      label 'k8-local-cluster'

    }

  }*/

  environment{
    registry = "jtrevinodev/guestbook"
    registryCredential = 'docker-hub-credential'
    app = ''

    image_tag = "${env.BRANCH_NAME}-${env.GIT_COMMIT}-${env.BUILD_NUMBER}"
  }

  stages {

    stage('Clone repository') {

      /*steps {
        git branch: 'master', credentialsId: 'github-key', url: 'git@github.com:jtrevinodev/guestbook-devops.git'

      }*/

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
          //pwd
          //sh 'docker build -t jtrevinodev/guestbook:prod src/php-redis'

          app = docker.build("jtrevinodev/guestbook", "src/php-redis")
        }
        //sh 'docker build -t jtrevinodev/guestbook:prod src/php-redis'

        //sh 'docker push jtrevinodev/guesbook:prod'



      }

      /*steps {

        sh 'docker build -t jtrevinodev/guestbook:prod src/redis-slave'

        sh 'docker push jtrevinodev/guestbook:prod'

      }*/

    }

    stage('Push container image to registry'){
      
      steps{

        script{
          docker.withRegistry('', registryCredential ) {
            //app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            //app.push("${env.BRANCH_NAME}-${env.GIT_COMMIT}-${env.BUILD_NUMBER}")
            app.push(image_tag)

          }
        }

        
        
      }
       
    }
    

    stage('Deploy to Kubernetes') {
      steps{
        script{
          sh('echo "Deploying to production environment"')

          def frontend_deployment = readFile "deploy/resources/frontend-deployment.yaml"
          frontend_deployment = frontend_deployment.replaceAll("image:.*", "image: jtrevinodev/guestbook:${image_tag}")
          writeFile file: "deploy/resources/frontend-deployment.yaml", text: frontend_deployment
          sh('cat deploy/resources/frontend-deployment.yaml')

          sh 'echo "Pushing deployment config to deployment repository"'

          withCredentials([sshUserPrivateKey(credentialsId: 'github-key', gitToolName: 'git-tool')]) {
            sh 'git config --global user.email "jtrevino.dev@gmail.com"'
            sh 'git config --global user.name "Jenkins pipeline"'
            sh 'git checkout -b dev'
            sh 'git add deploy/resources/frontend-deployment.yaml'
            sh 'git commit -m "image tag updated: ${image_tag}"'
            sh 'git push origin dev'
          }

        }
        
      }
      
      // agent {

      //   kubernetes {
      //     label 'k8-local-cluster'

      //   }

      // }
      

      // steps {

      //   kubernetesDeploy(

      //     configs: 'deploy/prod/',

      //     kubeconfigId: 'my-kubeconfig'

      //   )

      // }

    }

  }

}

