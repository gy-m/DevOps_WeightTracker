pipeline {
    agent {label "slave_build"}

    stages {
        
        stage('Downlod_to_BlubStorage') {
            steps {
                // TODO - understanding how to do it
                echo 'Downloding Repository from Jenkins to Blub storage'
            }
        }
            
        stage('Build') {
            
            // TODO: Create artifacts
            steps {
                    echo 'Building..'
                    script {
                    
                        // TODO - to be Fixed (must cereate new VM linux)
                        echo 'creating .env file'
                        sh '''
                        echo "# Host configuration
                        PORT=8080
                        HOST=0.0.0.0
                        NODE_ENV=development
                        HOST_URL=http://20.74.41.65:8080
                        COOKIE_ENCRYPT_PWD=superAwesomePasswordStringThatIsAtLeast32CharactersLong!
                        # Okta configuration
                        OKTA_ORG_URL=https://dev-19252361.okta.com
                        OKTA_CLIENT_ID=0oakcgiuqSqdOXynT5d6
                        OKTA_CLIENT_SECRET=3QYagrVr4BIOV5Z3Bv1ldnhYfbwWc95a5vaU7cLk
                        # Postgres configuration
                        PGHOST=10.0.0.20
                        PGUSERNAME=postgres
                        PGDATABASE=postgres
                        PGPASSWORD=p@ssw0rd42
                        PGPORT=5432" > .env
                        '''
                        
                    }
            }
        }
        
        stage('Deploy') {
            
            steps {
                echo 'Deploying....'
                
                sh 'curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -'
                sh 'sudo apt-get install -y nodejs'
                sh 'sudo apt-get install -y build-essential'
                //sh 'cd workspace/pipeline-build'
                sh 'npm install'
                sh 'npm run initdb'
                sh 'sudo npm install pm2 -g'                                 // install pm2
                sh 'pm2 start npm -- run dev'                           // run "npm run dev" as a service in the background using pm2
                //sh 'npm run dev'
                
                echo 'Finished building process'
                
            }
        }
        stage('Test') {
            
            steps {
                echo 'Testing..'
            }
        }
    }

    post {
        always {
            echo 'Creating tar.gz file for artifacts'
            sh 'tar -zcvf /home/proj_admin/my_archive.tar.gz /home/proj_admin/workspace/pipeline-build'
            archiveArtifacts artifacts: 'my_archive.tar.gz', onlyIfSuccessful: true
        }
    }
}
