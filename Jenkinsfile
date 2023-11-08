pipeline {
    agent {
        label 'jenkins-slave'
    }
    stages {
        stage('Get Source') {
            steps {
                echo "1.Clone Repo Stage"
                git credentialsId: 'GithubAccess', url: 'https://github.com/jesp1/app-repo'
                script {
                    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    repo_name = '703175063703.dkr.ecr.us-west-2.amazonaws.com'
                    app_name = 'gitops-app-demo'
                }
            }
        }
        stage('Build Image') {
            steps {
                echo "2.Build Docker Image Stage"
                sh "docker build --network host -t ${repo_name}/${app_name}:latest ."
                sh "docker tag ${repo_name}/${app_name}:latest ${repo_name}/${app_name}:${build_tag}"
            }

        }
        stage('Push Image') {
            steps {
                echo "3.Push Docker Image Stage"
                withDockerRegistry(credentialsId: 'ecr:us-west-2:ecr-repo', url: 'https://703175063703.dkr.ecr.us-west-2.amazonaws.com') {
                    sh "docker push ${repo_name}/${app_name}:latest"
                    sh "docker push ${repo_name}/${app_name}:${build_tag}"
                }
            }
        }
    }
}
