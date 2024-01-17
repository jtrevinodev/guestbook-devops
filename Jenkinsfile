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
            app.push("${env.BRANCH_NAME}-${env.GIT_COMMIT}-${env.BUILD_NUMBER}")

          }
        }

        
        
      }
       
    }
    

    stage('Deploy to Kubernetes') {
      steps{
        script{
          echo "Deploying to production environment"
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

