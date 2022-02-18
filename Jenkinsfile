// Jenkins multi-branch pipeline example

pipeline{
    agent any

    options{
        buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '30'))
        timeout(time: 5, unit: 'MINUTES')
    }

    stages{
        stage('cleanup workspace'){
            steps{
                cleanWs()
                sh '''
                    echo 'Workspace cleaned'
                '''
            }
        }
        stage('code checkout'){
            steps{
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/master']],  
                    userRemoteConfigs: [[url: 'https://github.com/chandras-xl/multi-branch-pipeline-demo.git']]])
            }
        }
        stage('Unit testing'){
            steps{
                sh '''
                    echo 'Performing unit test'
                '''
            }
        }
        stage('Code analysis'){
            steps{
                sh '''
                    echo 'Performing code analysis'
                '''
            }
        }
        stage('Build and deploy'){
            when{
                branch 'develop'
            }
            steps{
                sh '''
                    echo 'Building and deploying the code'
                '''
            }
        }
    }
    post{
        success{
            slackSend channel: '#jenkinsci',
                      color: 'good',
                      message: "Executed job ${currentBuild.fullDisplayName} successfully"
        }
        failure{
            slackSend channel: '$jenkinsci',
                      color: 'danger',
                      message: "Failed to execute job ${currentBuild.fullDisplayName}"
        }
    }
}