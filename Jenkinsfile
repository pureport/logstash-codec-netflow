pipeline {
    agent {
      docker {
        image 'jruby:9'
        args '-u root'
      }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh """
                    pwd
                    bundle install
                """
            }
        }
        stage('Test') {
            steps {
                sh """
                    bundle exec rspec
                """
            }
        }
        stage('Package') {
            steps {
                sh """
                    gem build logstash-codec-netflow.gemspec
                """
            }
        }
    }
    post {
        success {
            slackSend(color: '#30A452', message: "SUCCESS: <${env.BUILD_URL}|${env.JOB_NAME}#${env.BUILD_NUMBER}>")
        }
        unstable {
            slackSend(color: '#DD9F3D', message: "UNSTABLE: <${env.BUILD_URL}|${env.JOB_NAME}#${env.BUILD_NUMBER}>")
        }
        failure {
            slackSend(color: '#D41519', message: "FAILED: <${env.BUILD_URL}|${env.JOB_NAME}#${env.BUILD_NUMBER}>")
        }
    }
}
