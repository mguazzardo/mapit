pipeline {

  agent any

  stages {

    stage('Build') {

      when {

        expression {

          openshift.withCluster() {

            return !openshift.selector('bc', 'mapit').exists();

          }

        }

      }

      steps {

        script {

          openshift.withCluster() {

            //openshift.newApp('redhat-openjdk18-openshift:1.1~https://github.com/siamaksade/mapit-spring.git')
	    openshift.newApp( 'https://github.com/openshift/ruby-hello-world' ).narrow('bc')
          }

        }

      }

    }

  }

}
