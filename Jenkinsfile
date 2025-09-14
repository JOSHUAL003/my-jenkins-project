pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myjenkinsproject:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JOSHUAL003/my-jenkins-project.git'
            }
        }

       stage('Build & Unit test') {
           steps {
        sh 'pip install -r requirements.txt'
        sh 'python3 -m pytest --maxfail=1 --disable-warnings -q'
    
            }
        }

      stage('Docker Build') {
          steps {
        sh 'DOCKER_BUILDKIT=0 docker build -t myjenkinsproject:latest .'
              sleep 60
                }
        }

        stage('Run & Smoke Test') {
            steps {
                sh 'docker run -d -p 5000:5000 --name myapp1 $DOCKER_IMAGE'
                sleep 100

                sh 'curl -f http://localhost:5000/ || (echo "Smoke test failed!" && exit 1)'
            }
        }
        post {
	always {
		emailtxt (
    subject: '$DEFAULT_SUBJECT',
    body: '$DEFAULT_CONTENT',
    to: '$DEFAULT_RECEIPIENT',
    attachmentsPattern: 'testFile.*'
)
}
}
    }
}
