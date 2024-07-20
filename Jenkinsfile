pipeline {
    agent any

    environment {
        DOCKER_HUB_LOGIN = credentials('docker-hub')
        VERSION = sh(script: 'jq --raw-output .version package.json', returnStdout: true).trim()
        REPO = sh(script: 'basename `git rev-parse --show-toplevel`', returnStdout: true).trim()
        REGISTRY = credentials('registry-hub')
        DOCKER_ID = credentials('DOCKER_ID')
        SNYK_CREDENTIALS = credentials('snyk-token')
    }

    stages {
        /*
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
        */
        /*  
        stage ('Security SAST') {
          parallel {
             stage('Gitleaks-Scan') {
                    agent {
                        docker {
                            image 'zricethezav/gitleaks'
                            args '--entrypoint="" -u root -v ${WORKSPACE}:/src'
                        }
                    }                    
                    steps {
                        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                            script {
                                sh "gitleaks detect --verbose --source . -f json -r /src/report_gitleaks.json"
                                stash includes: 'report_gitleaks.json', name: 'report_gitleaks.json'
                            }
                        }
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
            }
        
        }

        */
        //stage('Build') {
        //    steps {
        //        echo 'Compilando el código...'
        //    }
        //}

         stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t $DOCKER_ID/$REPO:$VERSION ."
                }
            }
        }
         stage('Trivy-Scan') {
            agent {
                docker {
                    image 'aquasec/trivy:0.48.1'
                    args '--entrypoint="" -u root -v /var/run/docker.sock:/var/run/docker.sock -v ${WORKSPACE}:/src'
                }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    
                    script {
                        sh "trivy image --timeout 10m --format json --ignore-unfixed  --debug  $DOCKER_ID/$REPO:$VERSION > report_trivy.json"
                        stash includes: 'report_trivy.json', name: 'report_trivy.json'
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

