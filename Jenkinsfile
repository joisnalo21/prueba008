pipeline {
    agent any  // Indica que se puede ejecutar en cualquier nodo de Jenkins
    
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                git 'https://github.com/joisnalo21/prueba008.git'
            }
        }
        
        stage('Instalar Dependencias') {
            steps {
                // Usa Docker para instalar las dependencias de Composer
                script {
                    docker.image('composer:latest').inside {
                        sh 'composer install --no-dev --prefer-dist'
                    }
                }
            }
        }

        stage('Ejecutar Pruebas Unitarias') {
            steps {
                // Ejecuta las pruebas unitarias (esto asume que tienes PHPUnit o algo similar configurado)
                script {
                    sh 'php vendor/bin/phpunit --configuration phpunit.xml'
                }
            }
        }
    }

    post {
        success {
            // Se ejecuta si el pipeline fue exitoso
            echo 'Pipeline ejecutado correctamente.'
        }
        failure {
            // Se ejecuta si el pipeline falla
            echo 'Hubo un error en la ejecución del pipeline.'
        }
    }
}

