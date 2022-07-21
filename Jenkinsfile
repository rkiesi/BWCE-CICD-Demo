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
        sh 'docker build -t k3d-myregistry.localhost:12345/helloworld:latest .'
      }
    }

#    stage('Push Image to Registry') {
#      steps {
#        echo 'Pushing image to registry'
#        sh 'docker push k3d-myregistry.localhost:12345/helloworld:latest'
#      }
#    }

#    stage('Deploy') {
#      steps {
#        echo 'Deploy App on kubernetes'
#        sh '''k3d kubeconfig get mycluster > config.yaml
#
#        kubectl apply -f manifest.yaml --kubeconfig=config.yaml'''
#        echo 'Deployed successfully'
#      }
#    }

  }
  tools {
    maven 'maven382'
  }
}
