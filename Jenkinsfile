pipeline {
    agent any
    environment {
    SCANNER_HOME = tool 'sonar-scanner' 
}

    tools {
        nodejs 'node16'
        maven 'maven'
    }
    stages {
        stage('Compilación') {
            steps {
                echo 'Iniciando la compilación del Back-End...'
                dir('Back-End') {
                    sh 'mvn clean package'
                }
                
                echo 'Iniciando la compilación del Front-End...'
                dir('Front-End') {
                    sh 'npm install'
                    sh 'npm run build -- --source-map=false'
                }
            }
        }
        stage('Análisis con SonarQube') {
    steps {
        withSonarQubeEnv('sonar') {
            
            echo 'Analizando el código Java (Back-End)...'
            dir('Back-End') {
                sh 'mvn sonar:sonar -Dsonar.projectKey=ITS-Backend -Dsonar.projectName="ITS Back-End"'
            }
            
            echo 'Analizando el código web (Front-End)...'
            dir('Front-End') {
                sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=ITS-Frontend -Dsonar.projectName=\"ITS Front-End\""
            }
        }
    }
}
        stage('Quality gate') {
    steps {
        timeout(time: 1, unit: 'HOURS') {
            waitForQualityGate abortPipeline: true
        }
        }
    }
    stage('Subida a Nexus') {
            steps {
                echo 'Subiendo archivo .jar a Nexus...'
            }
    }

        stage('Docker Build') {
            steps {
                echo 'Creando imagen de Docker...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Desplegando la aplicación...'
            }
        }
    }
}
