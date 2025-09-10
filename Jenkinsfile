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
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run & Smoke Test') {
            steps {
                sh 'docker run -d -p 5000:5000 --name myapp $DOCKER_IMAGE'
                sh 'sleep 5'
                sh 'curl -f http://localhost:5000/ || (echo "Smoke test failed!" && exit 1)'
            }
        }
    }
}
