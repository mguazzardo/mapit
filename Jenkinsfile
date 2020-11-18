pipeline {

  agent any

  stages {

    stage('Build') {


      steps {

        script {

          openshift.withCluster() {

	    openshift.newApp( 'https://github.com/openshift/ruby-hello-world' ).narrow('bc')
          }

        }

      }

    }

  }

}
