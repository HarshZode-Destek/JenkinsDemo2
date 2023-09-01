pipeline{
    agent any
    environment{
        staging_server="10.128.0.2"
    }
    stages{
        stage('Deploy to Remote'){
            steps{
                 sshagent(['JenkinsServer']) {
                  sh '''
                    for fileName in $(find ${WORKSPACE} -type f -mmin -10 | grep -v ".git" | grep -v "Jenkinsfile")
                    do
                    echo ${fileName}
                    fil=$(echo ${fileName} | sed 's/'"${JOB_NAME}"'/ /' | awk {'print $2'})
                    directory=$(dirname ${fileName} | sed 's/'"${JOB_NAME}"'/ /' | awk {'print $2'})
                    ssh root@${staging_server} "mkdir -p /var/www/html/${directory}"
                    // scp -r ${WORKSPACE}${fil} root@${staging_server}:/var/www/html${fil} || scp -r ${WORKSPACE}${fil} root@${staging_server}:/var/www/html${fil}' 
                    done
                '''
                }
                
            }
        }
    }
}
