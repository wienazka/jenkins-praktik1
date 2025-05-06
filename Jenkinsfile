pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }
    stages {
        stage('install Depedencies') {
            steps{
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Test') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "Build SUCCESS on '${env.BRANCH_NAME}'\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369147196364292137/TZzku3dcWQzOmrsVWuPwg956m2Rk3xu8UVSzSa-lAgIxXmts3DwEWpDef510OlQr7Ypz'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "Build F on '${env.BRANCH_NAME}'\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369147196364292137/TZzku3dcWQzOmrsVWuPwg956m2Rk3xu8UVSzSa-lAgIxXmts3DwEWpDef510OlQr7Ypz'
                )
            }
        }
    }
}
