pipeline {
agent any

environment {
    DOCKER_IMAGE = "rajeshtutta123/isro_project_img"
}

stages {

    stage('GIT CHECKOUT') {
        steps {
            git branch: 'main',
            credentialsId: 'rajeshcred',
            url: 'https://github.com/rajeshtutta/Isro1.git'
        }
    }

    stage('BUILD') {
        steps {
            sh 'mvn clean package -DskipTests -X'
        }
    }

    stage('Docker Build') {
        steps {
            sh 'docker build -t $DOCKER_IMAGE:latest .'
        }
    }

    stage('Docker Login') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'echo $PASS | docker login -u $USER --password-stdin'
            }
        }
    }

    stage('Push Image') {
        steps {
            sh 'docker push $DOCKER_IMAGE:latest'
        }
    }

    stage('Deploy Container') {
        steps {
            sh '''
            docker rm -f isro_cont || true
	    docker run -d -p 1996:8080 --name isro_cont $DOCKER_IMAGE:latest
            '''
        }
    }

}

}
