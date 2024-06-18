pipeline{
    agent any

    stages{
        stage('zip the file'){
            steps{
                sh 'zip ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
            }
        }
        stage('upload artifacts to jfrog'){
            steps{
                curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd \
                -T <PATH_TO_FILE> \
                "http://ec2-18-209-224-128.compute-1.amazonaws.com:8081/artifactory/ansible/<TARGET_FILE_PATH>"
            }
        }
        stage{'publish to ansible server'}{
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansibleserver', \
                transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ls', execTimeout: 120000,\
                flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', \
                remoteDirectory: '/home/ec2-user', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'Ansible-${BUILD_ID}.zip')], \
                usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}