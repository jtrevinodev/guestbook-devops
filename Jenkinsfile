pipeline {

  agent {

    kubernetes {

      label 'k8-local-cluster'

    }

  }

  stages {

    stage('Clone repository') {

      steps {
        git branch: 'master', credentialsId: 'github-key', url: 'git@github.com:jtrevinodev/guestbook-devops.git'

      }

    }

    

    stage('Build and push Docker image') {

      steps {

        sh 'docker build -t jtrevinodev/guestbook:prod src/php-redis'

        sh 'docker push jtrevinodev/guesbook:prod'

      }

      /*steps {

        sh 'docker build -t jtrevinodev/guestbook:prod src/redis-slave'

        sh 'docker push jtrevinodev/guesbook:prod'

      }*/

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

