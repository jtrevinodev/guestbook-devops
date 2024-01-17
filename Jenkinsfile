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
          //git credentialsId: 'github-key', url: 'git@github.com:jtrevinodev/guestbook-devops.git'
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
          
          sh 'echo "Clonning deployment repository"'

          // withCredentials([gitUsernamePassword(credentialsId: 'github-accesstoken', gitToolName: 'Default')]){
          //   git clone 
          // }

          //docker.image('argoproj/argo-cd-ci-builder:v1.0.0').inside {
            dir("deploy") {
            git credentialsId: 'github-key', url: 'git@github.com:jtrevinodev/guestbook-devops-deploy.git'
            sh('pwd')
            sh('ls')
            
            def frontend_df = "guestbook-devops-deploy/base/resources/frontend-deployment.yaml"
            def frontend_deployment = readFile frontend_df
            frontend_deployment = frontend_deployment.replaceAll("image:.*", "image: jtrevinodev/guestbook:${image_tag}")
            writeFile file: frontend_df, text: frontend_deployment
            sh('cat ${frontend_df}')

            sh 'echo "Pushing deployment config to deployment repository"'

            
            sh 'git config --global user.email "jtrevino.dev@gmail.com"'
            sh 'git config --global user.name "Jenkins pipeline"'
            //sh 'git checkout master'
            sh 'git add ${frontend_df}'
            sh 'git commit -m "image tag updated: ${image_tag}"'
            sh 'git push origin master'

            }
          //}

          
          

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

