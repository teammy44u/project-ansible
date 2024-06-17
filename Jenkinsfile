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
                sh 'curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd \
                -T /home/ec2-user/jenkins/workspace/ansible-playbook \
                "http://ec2-18-209-224-128.compute-1.amazonaws.com:8081/artifactory/ansible-playbook/playbooks_${BUILD_ID}.zip"'
            }
        }
    }
}