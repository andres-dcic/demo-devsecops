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

        stage('Install Dependencies') {
            agent {
                docker {
                    image 'node:16-alpine'
                    args '-u root:root'
                }
            }
            steps {
                script {
                    sh 'npm install'
                }
            }
        }   
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
                 catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    script {
                    sh "snyk test --json --file=package.json --severity-threshold=high --print-deps --print-deps-uses --print-vulnerabilities --print-trace --print-all-environment --json-file-output=report_snyk.json"
                    stash includes: 'report_snyk.json', name: 'report_snyk.json'
                     }
                  }
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

