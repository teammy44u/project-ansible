pipeline{
    agent any

    stages{
        stage('zip the file'){
            steps{
                sh 'rm -rf *.zip || echo ""'
                sh 'zip -r ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
            }
        }
        stage('upload artifacts to jfrog'){
            steps{
                sh 'curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd \
                -T  ansible-${BUILD_ID}.zip \
                "http://ec2-54-84-11-183.compute-1.amazonaws.com:8081/artifactory/ansible2/ansible-${BUILD_ID}.ZIP"'
            }
        }
        stage('publish to ansible server'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansibleserver', transfers: \
                [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'unzip ansible-${BUILD_ID}.zip; rm -rf ansible-${BUILD_ID}.zip', \
                execTimeout: 121000, \
                flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', \
                remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '', \
                sourceFiles: 'ansible-${BUILD_ID}.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}