pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        echo 'Checkout code from source control (GitHub)'
      }
    }

    stage('Build EAR') {
      steps {
        echo 'Building EAR'
        sh 'mvn -f CICD-Demo.parent/pom.xml package'
        sh 'pwd'
      }
    }

    stage('Build Image') {
      steps {
        echo 'Start creating container image'
        sh 'docker build -t k3d-myregistry.localhost:12345/helloworld:1.0.7 .'
     // sh 'docker build -t k3d-myregistry.localhost:12345/helloworld:testing .'
      }
    }

    stage('Push Image to Registry') {
      steps {
        echo 'Pushing image to registry'
        sh 'docker push k3d-myregistry.localhost:12345/helloworld:1.0.7'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy App on kubernetes'
        sh '''k3d kubeconfig get mycluster > config.yaml

        kubectl apply -f k8s/app-manifest.yaml --kubeconfig=config.yaml'''
        echo 'Deployed successfully'
      }
    }

  }
  tools {
    // How to configure maven in Jenkins? -> https://www.youtube.com/watch?v=V-VFrYF_Z1Y
    maven 'maven382'
  }
}
