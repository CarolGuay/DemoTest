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
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/CarolGuay/DemoTest.git'
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

         stage('Dependency-Track Analysis') {
            steps {
                dependencyTrack(
                    project: '<08404479-dca3-4314-9891-8e657a49bc3d>',
                    application: '<DemoTest>',
                    scanPath: '.',
                    format: 'JSON'
                )
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