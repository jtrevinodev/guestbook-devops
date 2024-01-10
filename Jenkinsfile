pipeline {

  agent {

    kubernetes {

      label 'k8-local-cluster'

    }

  }

  

  stages {

    stage('Clone repository') {

      steps {
	
        git 'git@github.com:jtrevinodev/guestbook-devops.git'

      }

    }

    

    stage('Build and push Docker image') {

      steps {

        sh 'docker build -t my-image:latest .'

        sh 'docker push my-image:latest'

      }

    }

    

    stage('Deploy to Kubernetes') {

      steps {

        kubernetesDeploy(

          configs: 'kubernetes/deployment.yaml',

          kubeconfigId: 'my-kubeconfig'

        )

      }

    }

  }

}

