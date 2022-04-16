pipeline {
    agent any 

    environment {
        ANSIBLE_REPO = '/var/lib/jenkins/workspace/ansible_master'
        WEBHOOK = credentials('JENKINS_DISCORD')
    }

    //triggering periodically so the code is always present
    // run every friday at 3AM
    // triggers { cron('0 3 * * 5') }

    stages {
        // make new ansible code available to drake
        stage('franken-printer') {
            when { branch 'master' }
            steps {
                sh 'ansible-playbook ${ANSIBLE_REPO}/deploy/deploy_klipper.yml'
            }
        }

        // make new ansible code available to drake
        stage('voron') {
            when { branch 'voron' }
            steps {
                sh 'ansible-playbook ${ANSIBLE_REPO}/deploy/deploy_klipper_voron.yml'
            }
        }
    }
    post {
        always {
            discordSend \
                description: "${JOB_NAME} - build #${BUILD_NUMBER}", \
                // footer: "Footer Text", \
                // link: env.BUILD_URL, \
                result: currentBuild.currentResult, \
                // title: JOB_NAME, \
                webhookURL: "${WEBHOOK}"
        }
    }
}

