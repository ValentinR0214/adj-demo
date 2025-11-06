pipeline {
    agent any

    stages{
        // parar todos los servicios
        stage('Parando los servicios') {
            steps{
                sh ''' 
                    docker compose -p adj-demo down || true
                '''
            }
        }

        // Eliminar imagenes anteriores
        stage('Borrando imagenes antiguas') {
            steps{
                sh '''
                    IMAGES=$(docker images --filter "label=com.docker.compose.project=adj-demo" -q)
                    if [-n '$IMAGES']; then
                        docker images rmi $IMAGES
                    else
                        echo "No hay imagenes para borrar"
                    fi
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
                sh '''
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