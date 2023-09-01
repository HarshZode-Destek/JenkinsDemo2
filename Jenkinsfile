pipeline {
    agent any
    environment {
        staging_server = "10.128.0.2"
        remoteDir = "/var/www/html"
    }
    stages {
        stage('Deploy to Remote') {
            steps {
                sshagent(['JenkinsServer']) {
                    sh '''
                        set -e
                        rsync -avh --progress --update --exclude=".git" --exclude="Jenkinsfile" ${WORKSPACE}/ --delete root@${staging_server}:${remoteDir}/website
                    '''
                }
            }
        }
        stage('Cleanup') {
            steps {
                sshagent(['JenkinsServer']) {
                    sh '''
                        set -e
                        find ${remoteDir} -type f ! -newermt now -delete
                        find ${remoteDir} -type d -empty -delete
                    '''
                }
            }
        }
    }
}
