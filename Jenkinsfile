pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/2206-devops-batch/AJNJ-project2.git'

                // Build a docker tag
                sh 'docker build -t kubgroup/p1_kubgroup:latest .'
                
                sh 'python3 -m pip install -r requirements.txt'
                
            }
        }
        stage('Test'){
            steps{
                // Run pytest
                sh "python3 -m pytest app-test.py"
            }
        }
        // stage('SonarQube analysis') {
        //     steps{
        //         withSonarQubeEnv(credentialsId: '0ac9ce6aac7acebcec0254501ee4108a117adfb1', installationName: 'SonarCloud') { // You can override the credential to be used
        //             sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
        //         }
        //     }
        //}
        stage('Deploy') {
            steps{
                withCredentials([string(credentialsId: 'grouppass', variable: 'dockhubpass')]) {
                    sh "docker login -u kubgroup -p ${dockhubpass}"
                }                 
                sh 'docker push kubgroup/p1_kubgroup:latest'
            }
        }
        stage('Run') {
            steps{
                
                sh 'python3 -m flask run --host="0.0.0.0" &'
            }
        
        }
    }
    
    post {
        always{
            discordSend description: '', enableArtifactsList: true, footer: '', image: '', link: 'http://ec2-35-175-147-59.compute-1.amazonaws.com:8080/', result: currentBuild.currentResult, scmWebUrl: '', showChangeset: true, thumbnail: '', title: 'KubBot 2.0', webhookURL: 'https://discord.com/api/webhooks/994670791381766194/1puwuCYH5ZoyNrxHo8CMgPx8lDhfI7YWWV2BbR78hY04QYxR9SShZuZehj1hezwpHOt5'
        }
    }
}
