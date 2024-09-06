pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        string(name: 'ENVIRONMENT', defaultValue: 'staging', description: 'Deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests')
    }

    environment {
        MY_ENV_VAR = 'value'
        DEP_TRACK_URL = 'http://localhost:8081'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio y selecciona la rama especificada
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/CarolGuay/Analisis_vulnerabilidades.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building branch ${params.BRANCH_NAME}..."
                // Aquí puedes agregar los comandos para construir tu aplicación
                sh 'echo "Build step"'
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS }
            }
            steps {
                echo 'Testing...'
                // Aquí puedes agregar los comandos para ejecutar tus pruebas
                sh 'echo "Test step"'
            }
        }

         stage('Dependency Analysis') {
            steps {
                echo 'Running Dependency Track analysis...'
                // Ejecutar el análisis de dependencias usando Dependency Track
                sh '''
                # Comando para enviar el archivo de dependencias a Dependency Track
                dependency-track -u $DEP_TRACK_URL -a /path/to/your/analysis-file.json
                '''
            }
        }

        stage('Archive Results') {
            steps {
                echo 'Archiving Dependency Track results...'
                // Archivar el archivo de resultados generados
                archiveArtifacts artifacts: '**/path/to/your/analysis-file.json', allowEmptyArchive: true
            }
        }    

        stage('Deploy') {
            steps {
                echo "Deploying to environment ${params.ENVIRONMENT}..."
                // Aquí puedes agregar los comandos para desplegar tu aplicación
                sh 'echo "Deploy step"'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Aquí puedes agregar comandos que se ejecutarán siempre, como la limpieza
        }

        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed!'
        }

        unstable {
            echo 'Pipeline is unstable.'
        }
    }
}