pipeline {
  agent any
  stages {
    stage('Ongoing') {
      parallel {
        stage('msg1') {
          steps {
            sh 'echo "Launchin the script....."'
          }
        }

        stage('msg2') {
          steps {
            sh 'echo "A script is ongoing"'
          }
        }

      }
    }

    stage('virtualenv') {
      steps {
        sh 'python3 -m venv .venv'
        sh '''#!/bin/bash
              source .venv/bin/activate 
         '''
      }
    } 
    
    stage('Dependencies') {
      steps {
         sh '''#!/bin/bash
              source .venv/bin/activate 
              pip install -r requirements.txt 
         '''
      }
    }

    stage('Plan Testing') {
      parallel {
        stage('Setup Plan') {
          steps {
            sh '''#!/bin/bash
                  source .venv/bin/activate 
                  pytest --setup-plan --disable-warnings
            '''
          }
        }

        stage('Collect Tests') {
          steps {
            sh '''#!/bin/bash
                  source .venv/bin/activate 
                  pytest --disable-warnings --collect-only
            '''
          }
        }

      }
    }

    stage('Test') {
      steps {
         sh '''#!/bin/bash
              source .venv/bin/activate 
              coverage run -m pytest --disable-warnings -v
         '''
      }
    }

    stage('Coverage Test') {
      steps {
         sh '''#!/bin/bash
              source .venv/bin/activate 
              coverage report
              coverage xml
              pytest --junitxml=report.xml --disable-warnings -v
         '''
      }
    }

    stage('Closing') {
      steps {
        sh 'echo "ending the script"'
        sh 'ls'
      }
    }
  }

  post {
      always {
          junit 'report.xml'
      }
  }
}
