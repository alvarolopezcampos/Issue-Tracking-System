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
        stage('Compile') {
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
        stage('SonarQube Analysis') {
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
        stage('Nexus Upload') {
    steps {
        echo 'Subiendo archivo .jar a Nexus...'
        nexusArtifactUploader(
            nexusVersion: 'nexus3',
            protocol: 'http',
            nexusUrl: 'localhost:8081', 
            groupId: 'com.ismail',
            version: '0.0.1-SNAPSHOT',
            repository: 'maven-snapshots',
            credentialsId: 'nexus-credentials', 
            artifacts: [
                [artifactId: 'issuetracking', 
                 file: 'Back-End/target/issuetracking-0.0.1-SNAPSHOT.jar', 
                 type: 'jar']
            ]
        )
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
