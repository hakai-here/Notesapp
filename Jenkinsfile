pipeline {
  parameters {
    booleanParam(name: 'CleanAllBuild', defaultValue: true, description: 'Toggle this value to clean the builds ')
    booleanParam(name: 'StopTheBuild',defaultValue: true,description: 'Toggle this value to Stop the current running build')
  }
  agent {
    label {
      label 'dev-slave-node'
      retries 2
    }
  }

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

    stage('Push') {
      steps {
        echo "Pushing the code to dockerhub"
        withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notesapp ${env.dockerHubUser}/notesapp:latest"
                sh "docker login -u ${env.dockerHubUser} -p '${env.dockerHubPass}'"
                sh "docker push ${env.dockerHubUser}/notesapp:latest"
                }
      }
    }

    stage('Deploy') {
      steps {
        echo "Deploying the code"
        sh "docker-compose down && docker-compose up -d"
      }
    }

  }

  post {
    always {
      script {
        if (params.CleanAllBuild) {
          def proc = "docker system prune -af"
          sh(returnStatus: true, script: proc)

        }
      }
    }
  }

}