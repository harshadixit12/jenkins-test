pipeline {
  agent any

  tools {nodejs "node"}

  stages {
    stage('Install Postman CLI') {
      steps {
        sh 'curl -o- "https://dl-cli.pstmn.io/install/linux64.sh" | sh'
        echo "Current workspace is ${env.WORKSPACE}"
        echo "Current value of path: ${PATH}"
        echo "Now, we login from path:"
        echo "Current workspace is ${env.WORKSPACE}"
        echo "Path: ${PATH}"
        sh '/usr/local/bin/postman login --with-api-key PMAK-6322a364df32ad74cdd88dcc-bc3042d8d717325f2e2001b5ce65765762'
        sh '/usr/local/bin/postman collection run "22912480-041d2995-a5b2-4ac0-9ea5-f640591d0cac"-e "22912480-8b6a2e58-45e3-4402-a912-7deea93062f3"'
        sh '/usr/local/bin/postman api lint --integration-id 122680'
      }
    }
  }
}
