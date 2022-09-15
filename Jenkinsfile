pipeline {
  agent any

  tools {nodejs "node"}

  stages {
    stage('Install Postman CLI') {
      steps {
        sh '''
          #!/bin/sh

          PREFIX='/usr/local'

          if [ "$(uname -m)" != "x86_64" ]
          then
            echo "Only x64 is supported at this time" 1>&2
            exit 1
          fi

          URL='https://dl-cli.pstmn.io/download/latest/linux64'

          # We should always make use a temporary directory
          # to not risk leaving a trail of files behind in the user's system
          TMP="$(mktemp -d)"
          clean() { rm -rf "$TMP"; }
          trap clean EXIT

          if command -v curl > /dev/null
          then
            curl --location --retry 10 --output "$TMP/postman-cli.tar.gz" "$URL"
          elif command -v wget > /dev/null
          then
            wget --output-document "$TMP/postman-cli.tar.gz" "$URL"
          else
            echo "You need either cURL or wget installed on your system" 1>&2
            exit 1
          fi

          tar --directory "$TMP" --extract --file "$TMP/postman-cli.tar.gz"

          # Don't use sudo(8) if we don't seem to need it
          RUN='sudo'
          if test -d "$PREFIX/bin" && test -w "$PREFIX/bin"
          then
            RUN='eval'
          elif test -d "$PREFIX" && test -w "$PREFIX"
          then
            RUN='eval'
          fi

          if [ "$RUN" = "sudo" ] && ! command -v "$RUN" > /dev/null
          then
            echo "You do not have enough permissions to write to $PREFIX and you do not have sudo(8) installed" 1>&2
            exit 1
          fi

          # Use install(1) rather than mkdir/cp
          "$RUN" install -d "$PREFIX/bin"
          "$RUN" install -m 0755 "$TMP/postman-cli" "$PREFIX/bin/postman"

          echo "The Postman CLI has been installed" 1>&2
          ls -r /usr/local/bin
          export PATH="$PATH:/usr/local/bin"
          postman login --with-api-key PMAK-6322a364df32ad74cdd88dcc-bc3042d8d717325f2e2001b5ce65765762
          postman collection run "22912480-041d2995-a5b2-4ac0-9ea5-f640591d0cac"-e "22912480-8b6a2e58-45e3-4402-a912-7deea93062f3"
          postman api lint --integration-id 122680
        '''
      }
    }
  }
}
