pipeline {
  agent any

  environment {
    INV = 'inventories/jenkins.ini'
  }

  stages {
    stage('Syntax check') {
      steps {
        sh 'ansible-playbook -i $INV website.yml --syntax-check'
      }
    }

    stage('Dry run') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-lab-key', keyFileVariable: 'SSH_KEY')]) {
          sh 'ansible-playbook -i $INV website.yml --private-key "$SSH_KEY" --check --diff'
        }
      }
    }

    stage('Approve') {
      steps {
        input message: 'Dry run looks good. Deploy to web1?'
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-lab-key', keyFileVariable: 'SSH_KEY')]) {
          sh 'ansible-playbook -i $INV website.yml --private-key "$SSH_KEY"'
        }
      }
    }

    stage('Smoke test') {
      steps {
        sh 'curl -fsS http://web1/ | grep -i "ansible"'
      }
    }
  }
}