pipipeline {
    agent { label 'JAVAJDKSPC' }

    stages {
        stage('git checkout') {
            steps {
                dir('INFRA_REPO') {
                    git url: 'https://github.com/sarveswarreddy81/INFRA_REPO.git', branch: 'main'
                }
            }
        }

        stage('terraform init') {
            steps {
                dir('INFRA_REPO') {
                    sh 'terraform init'
                }
            }
        }

        stage('terraform validate') {
            steps {
                dir('INFRA_REPO') {
                    sh 'terraform validate'
                }
            }
        }

        stage('terraform fmt') {
            steps {
                dir('INFRA_REPO') {
                    sh 'terraform fmt'
                }
            }
        }

        stage('infra scan') {
            steps {
                dir('INFRA_REPO') {
                    sh 'tfsec . || true'  // soft fail tfsec issues
                }
            }
        }

        stage('lint') {
            steps {
                dir('INFRA_REPO') {
                    sh 'tflint'
                }
            }
        }

        stage('terraform plan') {
            steps {
                dir('INFRA_REPO') {
                    sh 'terraform plan'
                }
            }
        }

        stage('terraform apply') {
            steps {
                dir('INFRA_REPO') {
                    sh 'terraform apply -auto-approve'
                }
            }
        }
    }
}
