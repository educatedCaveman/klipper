pipeline {
    agent any 

    environment {
        ANSIBLE_REPO = '/var/lib/jenkins/workspace/ansible_master'
    }

    //triggering periodically so the code is always present
    // run every friday at 3AM
    triggers { cron('0 3 * * 5') }

    stages {
        // make new ansible code available to drake
        stage('deploy klipper configs to octopi') {
            steps {
                sh 'ansible-playbook ${ANSIBLE_REPO}/deploy/deploy_klipper.yml'
            }
        }
    }
}

