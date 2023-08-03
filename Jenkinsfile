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
        withCredentials([string(credentialsId: 'BRKC-POSTMAN-V10-ENV', variable: 'POSTMAN_API_KEY')]) {
            sh 'postman login --with-api-key $POSTMAN_API_KEY'
        }
      }
    }

    stage('Running collection') {
      steps {
        sh 'postman collection run "${WORKSPACE}/postman/collections/Cat_Fact_API.json" --integration-id "139698-${JOB_NAME}${BUILD_NUMBER}"'
      }
    }

    stage('Running api linting') {
      steps {
        sh 'postman api lint c7d76376-2177-4b28-ac55-8bfed60a6e40 --integration-id 139698'
      }
    }
  }
}
