pipeline {
    agent any
    tools {
        nodejs 'node'
    }
    stages {
        stage('Compile') {
            steps {
                echo 'Iniciando la compilación del Back-End...'
                dir('Back-End') {
                    sh 'mvn clean package'
                }
                
                echo 'Iniciando la compilación del Front-End...'
                dir('Front-End') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Analizando código con SonarQube...'
            }
        }
        stage('Nexus Upload') {
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
