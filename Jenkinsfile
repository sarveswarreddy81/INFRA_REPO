pipeline{
    agent{
        label 'JAVAJDKSPC'
    }

    stages {
        stage('git checkout') {
            steps {
                git url:'https://github.com/sarveswarreddy81/INFRA_REPO.git',
                branch:'main'
            }
        }
        stage('terraform init'){
            steps{
                sh 'terraform init'
            }
        }
        stage('terraform validate'){
            steps{
                sh 'terraform validate'
            }
        }
        stage('terraform formate'){
            steps{
                sh 'terraform fmt'
            }
        }
        stage('infra scan'){
            steps{
                sh 'tfsec .'
            }
        }
        stage('lint'){
            steps{
                sh 'tflint'
            }
        }
        stage('terraform plan'){
            steps{
                sh 'terraform plan'
            }
        }
        stage('terraform apply'){
            steps{
                sh 'terraform apply -auto-approve'
            }
        }
    }
}