```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Descargando código del repositorio...'

                git branch: 'main',
                    url: 'https://github.com/ecredit-dev/arce-velasquez.git'
            }
        }

        stage('Verificar estructura') {
            steps {
                echo 'Verificando archivos del proyecto...'

                sh '''
                    echo "Contenido del proyecto:"
                    ls -la

                    echo "Verificando archivos requeridos..."

                    test -f "Prueba de HTML.HTML" || {
                        echo "ERROR: No existe el archivo HTML principal."
                        exit 1
                    }

                    test -f "E Credit logo y nombre (5).png" || {
                        echo "ERROR: No existe el logo."
                        exit 1
                    }

                    test -f "Multimedia/Screenshot.png" || {
                        echo "ERROR: No existe Screenshot.png."
                        exit 1
                    }

                    echo "Estructura verificada correctamente."
                '''
            }
        }

        stage('Validar HTML') {
            steps {
                echo 'Validando estructura básica del documento HTML...'

                sh '''
                    grep -qi "<HTML>" "Prueba de HTML.HTML" || {
                        echo "ERROR: No se encontró la etiqueta HTML."
                        exit 1
                    }

                    grep -qi "<HEAD>" "Prueba de HTML.HTML" || {
                        echo "ERROR: No se encontró la etiqueta HEAD."
                        exit 1
                    }

                    grep -qi "<BODY" "Prueba de HTML.HTML" || {
                        echo "ERROR: No se encontró la etiqueta BODY."
                        exit 1
                    }

                    grep -qi "<IMG" "Prueba de HTML.HTML" || {
                        echo "ERROR: No se encontraron imágenes."
                        exit 1
                    }

                    echo "HTML validado correctamente."
                '''
            }
        }

        stage('Verificar recursos') {
            steps {
                echo 'Verificando recursos utilizados por la página...'

                sh '''
                    grep -q 'E Credit logo y nombre (5).png' "Prueba de HTML.HTML" || {
                        echo "ERROR: El HTML no referencia correctamente el logo."
                        exit 1
                    }

                    grep -q 'Multimedia/Screenshot.png' "Prueba de HTML.HTML" || {
                        echo "ERROR: El HTML no referencia correctamente Screenshot.png."
                        exit 1
                    }

                    echo "Recursos verificados correctamente."
                '''
            }
        }

        stage('Publicar artefactos') {
            steps {
                echo 'Archivando archivos del proyecto...'

                archiveArtifacts artifacts: '''
                    Prueba de HTML.HTML,
                    E Credit logo y nombre (5).png,
                    Multimedia/Screenshot.png,
                    DawianStivenArceVelasquez.txt
                ''',
                allowEmptyArchive: false,
                fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente.'
        }

        failure {
            echo 'El pipeline presentó errores. Revisar los logs de Jenkins.'
        }

        always {
            echo "Build #${env.BUILD_NUMBER} - Estado: ${currentBuild.currentResult}"
        }
    }
}
```
