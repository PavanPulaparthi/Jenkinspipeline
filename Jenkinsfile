 pipeline {
  environment {
    registry = "pavanpulaparthi/studentapp"
    DOCKERHUB_CREDENTIALS=credentials('dockerhub-pavan')
  }
  agent any
  tools {
    maven 'maven-3.6.3' 
  }
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'main', url: 'https://github.com/PavanPulaparthi/studentapp.git'
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Building image') {
      steps{
        sh "sudo docker build -t $registry:$BUILD_NUMBER ."
      }
    }
   stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u pavanpulaparthi --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh "sudo docker push $registry:$BUILD_NUMBER"
			}
	}
    stage('run Docker image') {
      steps{
        sh "sudo docker container run -it -d --name studentapp -p 8081:8080  $registry:$BUILD_NUMBER"
      }
    }
  }
}