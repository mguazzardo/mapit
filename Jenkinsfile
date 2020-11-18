pipeline {

  agent {

      label 'maven'

  }

  stages {

    stage('Build App') {

      steps {

        sh "mvn install"

      }

    }

    stage('Create Image Builder') {

      when {

        expression {

          openshift.withCluster() {

            return !openshift.selector("bc", "mapit").exists();

          }

        }

      }

      steps {

        script {

          openshift.withCluster() {

            openshift.newBuild("--name=mapit", "--image-stream=sibdocker/openjdk18-openshift:latest", "--binary")

          }

        }

      }

    }

    stage('Build Image') {

      steps {

        script {

          openshift.withCluster() {

            openshift.selector("bc", "mapit").startBuild("--from-file=target/mapit-spring.jar", "--wait")

          }

        }

      }

    }

    stage('Promote to DEV') {

      steps {

        script {

          openshift.withCluster() {

            openshift.tag("mapit:latest", "mapit:dev")

          }

        }

      }

    }

    stage('Create DEV') {

      when {

        expression {

          openshift.withCluster() {

            return !openshift.selector('dc', 'mapit-dev').exists()

          }

        }

      }

      steps {

        script {

          openshift.withCluster() {

            //openshift.newApp("mapit:latest", "--name=mapit-dev").narrow('svc').expose()
	    openshift.newApp( 'https://github.com/openshift/ruby-hello-world' ).narrow('bc')
          }

        }

      }

    }

    stage('Promote STAGE') {

      steps {

        script {

          openshift.withCluster() {

            openshift.tag("mapit:dev", "mapit:stage")

          }

        }

      }

    }

    stage('Create STAGE') {

      when {

        expression {

          openshift.withCluster() {

            return !openshift.selector('dc', 'mapit-stage').exists()

          }

        }

      }

      steps {

        script {

          openshift.withCluster() {

            openshift.newApp("mapit:stage", "--name=mapit-stage").narrow('svc').expose()

          }

        }

      }

    }

  }

}
