pipeline{
   //agent any
   agent {label 'worker'}
   options{
    buildDiscarder(logRotator(numToKeepStr: '5'))
    disableConcurrentBuilds()
    retry(2)
    timeout(time: 1, unit: 'MINUTES')
   }
   parameters{
    string(name: 'BRANCH', defaultValue: 'master')
    choice(name: 'ENVIRONMENT', choices: ['Dev','QA', 'Prod'])
    booleanParam(name: 'TEST_CASES', defaultValue: false)
   }
   triggers{
    cron('H */4 * * *')
    pollSCM('H */4 * * *')
   }
   stages{
    stage('Build and Push'){
        steps{
            sh '''
            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 287353407661.dkr.ecr.us-east-1.amazonaws.com
            cd vote
            docker build -t 287353407661.dkr.ecr.us-east-1.amazonaws.com/shivanew-jenkins:v${BUILD_NUMBER} .
            docker push 287353407661.dkr.ecr.us-east-1.amazonaws.com/shivanew-jenkins:v${BUILD_NUMBER}
            '''
        }
    }
    stage('Deploy Stage'){
        steps{
            sh "sleep 10"
        }
    }
   }
    stage('Deploy to CXO'){
        when {
			environment name: 'ENVIRONMENT', value: 'prod'
			}	
    }
   }
   post{
    always{
        echo "Always Run"
    }
   }
}
