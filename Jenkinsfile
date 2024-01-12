pipeline {

  agent any
  /*agent {

    kubernetes {

      label 'k8-local-cluster'

    }

  }*/

  environment{
    registry = "jtrevinodev/guestbook"
    //registryCredential = '<dockerhub-credential-name>'
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
          echo "hola2"

          app = docker.build("jtrevinodev/guestbook:prod src/php-redis")
        }
        //sh 'docker build -t jtrevinodev/guestbook:prod src/php-redis'

        //sh 'docker push jtrevinodev/guesbook:prod'



      }

      /*steps {

        sh 'docker build -t jtrevinodev/guestbook:prod src/redis-slave'

        sh 'docker push jtrevinodev/guesbook:prod'

      }*/

    }

    stage('Push container image to registry'){
      
      steps{
        script{
          app.push("${env.BUILD_NUMBER}")
          app.push("prod")
        }
        
      }
       
    }
    

    /*stage('Deploy to Kubernetes') {

      steps {

        kubernetesDeploy(

          configs: 'deploy/prod/deployment.yaml',

          kubeconfigId: 'my-kubeconfig'

        )

      }

    }*/

  }

}

