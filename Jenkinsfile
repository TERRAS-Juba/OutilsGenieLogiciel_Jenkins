pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        always {
          mail(subject: 'Deploiement d\'une nouvelle version', body: 'Bonjour, Je vous informe qu\'une nouvelle version est disponible sur le github. Cordialement.', from: 'ij_terras@esi.dz', to: 'ij_terras@esi.dz')
        }

      }
      steps {
        bat './gradlew build'
        bat './gradlew javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/reports/**'
      }
    }

    stage('SonarQube analysis') {
      parallel {
        stage('SonarQube analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat './gradlew sonarqube'
            }

          }
        }

        stage('Test reporting') {
          steps {
            cucumber(fileIncludePattern: 'target/report.json', reportTitle: 'CucumberReports')
          }
        }

      }
    }

    stage('Quality gate') {
      post {
        failure {
          mail(subject: 'Erreur Quality Gate', body: 'Bonjour, le code soumis a un statut Quality Gates : failed. Cordialement.', from: 'ij_terras@esi.dz', to: 'ij_terras@esi.dz')
        }

      }
      steps {
        waitForQualityGate true
      }
    }

    stage('Publish') {
      steps {
        bat './gradlew publish'
      }
    }

  }
}