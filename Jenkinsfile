pipeline {
  agent any
  stages {
    stage('scm checkout') {
      steps {
        git(url: 'https://github.com/onkarjadhav8796/maven-project.git', branch: 'master', credentialsId: '56e65e19802dd7328268062590a8e7c2390265a4')
      }
    }

    stage('code test') {
      steps {
        withMaven(maven: 'maven') {
          sh 'mvn test'
        }

      }
    }

    stage('code package') {
      steps {
        withMaven(maven: 'maven') {
          sh 'mvn package'
        }

      }
    }

    stage('code install') {
      steps {
        withMaven(maven: 'maven') {
          sh 'mvn install'
        }

      }
    }

    stage('ssh tomcat') {
      steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'Tomcat', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/var/lib/tomcat/webapps', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }

    stage('deploy to tomcat') {
      steps {
        sshagent(credentials: ['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no */target/*.war ec2-user@13.235.99.177:/var/lib/tomcat/webapps'
        }

      }
    }

  }
}