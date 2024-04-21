pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Github hook trigger'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'MyAWS',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/PipelineTest/index.html s3://test-env-jenkins-20240416/')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                // Groovyスクリプト
                script {
                    // URLの変数
                    def url = 'https://test-env-jenkins-20240416.s3.ap-northeast-1.amazonaws.com/index1.html'
                    // ステータスコード変数
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' '$url'", returnStdout: true)

                    if (response == '200') {
                        echo 'Test OK'
                    } else {
                        echo response
                        error 'Test NG'
                    }
                        
                 }
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}
