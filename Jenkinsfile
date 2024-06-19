pipeline{
    agent any

    stages{
        stage('zip the file'){
            steps{
                sh '''
                    mkdir -p ansible-dev
                    rm -rf ansible-dev/*
                    shopt -s extglob
                    mv !(ansible-dev) ansible-dev/
                    '''
                dir ('/var/lib/jenkins/workspace/playbook-ansible2/ansible-dev'){
                    sh 'rm -rf *.zip || echo ""'
                    sh 'zip -r ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
                }
            }
        }
        stage('upload artifacts to jfrog'){
            steps{
                sh 'curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd \
                -T ansible-dev/ansible-${BUILD_ID}.zip \
                "http://ec2-54-84-11-183.compute-1.amazonaws.com:8081/artifactory/ansible2/ansible-${BUILD_ID}.ZIP"'
            }
        }
        stage('publish to ansible server'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansibleserver', transfers: \
                [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd ansible', \
                execTimeout: 121000, \
                flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', \
                remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '', \
                sourceFiles: 'ansible/ansible-${BUILD_ID}.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}