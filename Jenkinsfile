pipeline {
   agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: shell
                image: node
                command:
                - sleep
                args:
                - infinity
            '''
            defaultContainer 'shell'
        }
    }

  tools {nodejs "17.2.0"}

  
  stages {
    stage('Install Postman CLI') {
      steps {
        sh 'which curl'
        sh "echo $PATH"
        sh 'curl -o- "https://dl-cli.pstmn.io/install/linux64.sh" | sh'
      }
    }

    stage('Postman CLI Login') {
      steps {
        withCredentials([string(credentialsId: 'BRKC_CS_DEMO_KEY', variable: 'POSTMAN_API_KEY')]) {
            sh 'postman login --with-api-key $POSTMAN_API_KEY'
        }
      }
    }

    stage('Running collection') {
      steps {
        sh 'postman collection run "16901629-52c951a4-a262-45a7-939e-18ef0d7f25a4" --integration-id "123817-${JOB_NAME}${BUILD_NUMBER}"'
      }
    }

    stage('Running api linting') {
      steps {
        sh 'postman api lint --integration-id 123817'
      }
    }
  }
}
