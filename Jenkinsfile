pipeline {
  agent any
      stages {
        stage('Build') {
          steps {
            bat './gradlew build'
            bat './gradlew javadoc'
            archiveArtifacts 'build/libs/*.jar'
            archiveArtifacts 'build/reports/**'
          }
        }
      post{
        always{
          stage('Mail Notification') {
             steps {
                 mail(subject: 'Déploiement d\'une nouvelle version', body: 'Bonjour, Je vous informe qu\'une nouvelle version est disponible sur le github. Cordialement.', from: 'ij_terras@esi.dz', to: 'ij_terras@esi.dz')
               }
             }
           }
        }
      }
}