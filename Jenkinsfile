pipeline {
    agent { label 'JAVAJDKSPC' }

    stages {
        stage('Setup Tools') {
            steps {
                sh '''
                if ! command -v tfsec &> /dev/null; then
                    echo "Installing tfsec..."
                    curl -s https://raw.githubusercontent.com/aquasecurity/tfsec/master/scripts/install_linux.sh | bash
                    sudo mv tfsec /usr/local/bin/
                fi

                if ! command -v tflint &> /dev/null; then
                    echo "Installing tflint..."
                    curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash
                    sudo mv tflint /usr/local/bin/
                fi

                terraform --version
                tfsec --version
                tflint --version
                '''
            }
        }

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
                    sh 'tfsec . || true'
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
