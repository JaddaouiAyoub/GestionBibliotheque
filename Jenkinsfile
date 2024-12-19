pipeline {
    agent any
    environment {
        MAVEN_HOME = tool 'Maven'
        SONAR_PROJECT_KEY = 'library-management'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/JaddaouiAyoub/GestionBibliotheque.git'
            }
        }
        stage('Build') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn test'
            }
        }
        stage('Quality Analysis') {
            steps {
            withCredentials([string(credentialsId:'	sonar-token',variable:'	sonar-token')]){
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${MAVEN_HOME}/bin/mvn clean verify sonar:sonar\
                          -Dsonar.projectKey=library-management \
                          -Dsonar.projectName='library-management' \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.token=sqp_77184bb20899618f745968eb22c6533d43755af1
                        """
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement simulé réussi'
            }
        }
    }
    post {
        success {
            emailext to: 'jaddaouiayoub02@gmail.com',
                subject: 'Build Success',
                body: 'Le build a été complété avec succès.'
        }
        failure {
            emailext to: 'jaddaouiayoub02@gmail.com',
                subject: 'Build Failed',
                body: 'Le build a échoué.'
        }
    }
}
