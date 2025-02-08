pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tuusuario/laravel-app"
        DOCKER_REGISTRY = "docker.io"
    }

    stages {
        stage('Checkout Código') {
            steps {
                git branch: 'main', url: 'https://github.com/tuusuario/turepo.git'
            }
        }

        stage('Instalar Dependencias') {
            steps {
                sh 'composer install --no-dev --prefer-dist'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Ejecutar Pruebas Unitarias') {
            steps {
                sh './vendor/bin/phpunit'
            }
        }

        stage('Pruebas End-to-End con Selenium') {
            steps {
                sh 'php artisan serve &'
                sh 'selenium-server -port 4444 &'
                sh 'php artisan dusk'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Publicar Imagen Docker') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-cred', url: "$DOCKER_REGISTRY"]) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Desplegar en Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl rollout status deployment/laravel-app'
            }
        }

        stage('Monitoreo con Prometheus') {
            steps {
                sh 'kubectl apply -f k8s/prometheus-config.yaml'
            }
        }
    }

    post {
        success {
            echo 'Despliegue exitoso 🎉'
        }
        failure {
            echo 'Algo falló 💥'
        }
    }
}
