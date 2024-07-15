pipeline {
    agent any

    environment {
        //DOCKER_HUB_LOGIN = credentials('docker-hub')
        //VERSION = sh(script: 'jq --raw-output .version package.json', returnStdout: true).trim()
        //REPO = sh(script: 'basename `git rev-parse --show-toplevel`', returnStdout: true).trim()
        //REGISTRY = credentials('registry-hub')
        SNYK_CREDENTIALS = credentials('snyk-token2')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Compilando el código...'
            }
        }

        stage('Snyk Test') {
           steps {
               echo 'testing'
                snykSecurity(
                 snykInstallation: 'snyk-token2',
                 snykTokenId: 'snyk-token2',
                // place other optional parameters here, for example:
                    additionalArguments: '--all-projects --detection-depth=5'
                )
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

