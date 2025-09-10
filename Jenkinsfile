pipeline {
   agent any



   environment {
         DOCKER_IMAGE = "myjenkinsproject:latest"
    }


   stages {
   
       stage('Checkout') {
            steps {
		git branch: 'main', url: 'https://github.com/JOSHUAL003/my-jenkins-project.git'            }
        }

    stage('Build & Unit test') { 
       steps {
	   sh 'pip install -r reuirements.txt
	   sh 'pytest --maxfail=1 --disable-warnings -q'
	   }
      }

     stage('Docker Build') {
	steps {
	   sh 'docker run -d -p 5000:5000 --name myapp $DOCKER_IMAGE'
	   sh 'slepp 5'
	   sh 'curl -f http://localhost:5000/ || (echo "Smoke test failed!" && exit 1)'
	}
     }
	       stage('Cleanup') {
            steps {
                sh 'docker stop myapp || true'
                sh 'docker rm myapp || true'
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
