pipeline {
    agent any

    stages{
        // parar todos los servicios
        stage('Parando los servicios') {
            steps{
                // Usamos 'bat' y la sintaxis '|| exit /b 0' para ignorar errores
                bat ''' 
                    docker compose -p adj-demo down || exit /b 0
                '''
            }
        }

        // Eliminar imagenes anteriores
        stage('Borrando imagenes antiguas') {
            steps{
                // Usamos el bucle 'for /f' de Batch y la lógica de 'errorlevel'
                // Se cambió el filtro a "label=com.docker.compose.project=adj-demo"
                bat '''
                    for /f "tokens=*" %%i in ('docker images --filter "label=com.docker.compose.project=adj-demo" -q') do (
                        docker rmi -f %%i
                    )
                    if errorlevel 1 (
                        echo No hay imagenes por eliminar
                    ) else (
                        echo Imagenes eliminadas correctamente
                    )
                '''
            }
        }

        //Bajar la actualizacion
        stage('Actualizando...') {
            steps{
                checkout scm
            }
        }

        //Levantar y desplegar el proyecto
        stage('Levantando y desplegando el proyecto') {
            steps{
                // Usamos 'bat' para el comando final
                bat '''
                    docker compose up --build -d
                '''
            }
        }
    }

    post{
        success{
            echo 'Pipeline ejecutado exitosamente'
        }
        failure{
            echo 'Error en la ejecución del pipeline'
        }
        always{
            echo 'Fin del pipeline'
        }
    }
}