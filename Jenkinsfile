pipeline{
    agent any
    tools{
        maven "Maven"
    }
    stages{
        stage("Test"){
            steps{
                echo "Testing has begun."
                sh "mvn test"
            }
        }
        stage("Build"){
            steps{
                echo "Building has been started..."
                sh "mvn package"
            }
        }
        stage("Test-deploy"){
            steps{
                echo "Deployment to the test server is in process..."
                deploy adapters: [tomcat9(credentialsId: 'tomcat9details', path: '', url: 'http://192.168.1.8:8080')], contextPath: '/pacapp', war: '**/*.war'
            }
        }
        stage("permit"){
                input{
                    message "Do you want to proceed?"
                    ok "Yes, proceed"
                }
                steps{
                    echo "Proceeding"
                }
        }
        stage("Prod-deploy"){
            steps{
                echo "Permission has been granted and deployment onto the prod server has begun..."
                deploy adapters: [tomcat9(credentialsId: 'tomcat9details', path: '', url: 'http://172.22.133.98:8080')], contextPath: '/pacapp', war: '**/*.war'
            }
        }
    }
    post{
            always{
                echo "The task has been completed, please check the below status of the builds"
            }
            success{
                echo "War file has been deployed to prod successfully"
            }
            failure{
                echo "Deployment has been failed, please check the logs for the errors"
            }
    }
}
