pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Parando servicios') {
            steps {
                sh 'docker compose down || true'
            }
        }

        stage('Eliminando imágenes antiguas') {
            steps {
                sh '''
                    IMAGES=$(docker images --filter "label=com.docker.compose.project=demo" -q)
                    if [ -n "$IMAGES" ]; then
                        docker rmi -f $IMAGES
                    else
                        echo "No hay imágenes por borrar"
                    fi
                '''
            }
        }

        stage('Contruyendo y desplegando') {
            steps {
                sh 'docker compose up --build -d'
            }
        }
    }

    post {
        always { echo 'Pipeline finalizada.' }
        success { echo 'OK.' }
        failure { echo 'Falló.' }
    }
}
