pipeline {
 
  agent any

  stages {
    stage('Checkout') {
      steps {
        echo "checking out from git"
        git url: "https://github.com/syamsv/Notesapp.git", branch: "main"
      }
    }

    stage('Build') {
      steps {
        echo "Building the docker file"
        sh "docker build -t notesapp ."
      }
    }

    
    stage('Deploy') {
      steps {
        echo "Deploying the code"
        sh "docker-compose down && docker-compose up -d"
      }
    }

  }

}
