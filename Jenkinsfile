pipeline {
     agent any
     stages {
         stage('Lint HTML') {
              steps {
                    sh 'echo "LInting the *.html files"'
                    sh 'tidy -q -e *.html'
              }
         }
         stage('Upload to AWS') {
              steps {
                  withAWS(region:'us-west-2',credentials:'aws-static') {
                      sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-repo-project3')
                  }
              }
         }
          stage('Check the WebSite is Up') {
			steps {
				sh '''
					response=$(curl -s -o /dev/null -w "%{http_code}\n" https://static-jenkins-repo-project3.s3-us-west-2.amazonaws.com/index.html)
					if [ "$response" != "200" ]
					then
					 exit 1
					fi
				'''
			}
		}
     }
}
