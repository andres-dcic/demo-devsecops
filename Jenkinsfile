pipeline {
    agent any

    environment {
        //DOCKER_HUB_LOGIN = credentials('docker-hub')
        //VERSION = sh(script: 'jq --raw-output .version package.json', returnStdout: true).trim()
        //REPO = sh(script: 'basename `git rev-parse --show-toplevel`', returnStdout: true).trim()
        //REGISTRY = credentials('registry-hub')
        SNYK_CREDENTIALS = credentials('snyk-token')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Compilando el código...'
            }
        }

        stage('Snyk Test') {
           agent {
             docker {
             image 'snyk/snyk:node'
             args '--entrypoint="" -e SNYK_TOKEN=$SNYK_CREDENTIALS -u root:root -v ${WORKSPACE}:/src'
             }
            }      
           steps {
               echo 'testing'
               }
           }
        

        stage('Pruebas') {
            steps {
                echo 'Ejecutando pruebas...'
            }
        }

        stage('Despliegue') {
            steps {
                echo 'Desplegando la aplicación...'
            }
        }
    }
}

