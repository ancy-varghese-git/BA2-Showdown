pipeline {
  agent any
  stages {
  stage('Stage 1 - Initialize') {
      steps {
        script {
          echo 'Stage 1-we initialize'
		  echo "Path=> ${PATH}"
		  echo "WORKSPACE=> ${WORKSPACE}"
		  sh 'npm config ls'
        }
      }
    }
  stage('Stage 2 - Angular Build') {
      steps {
        script {
          echo 'Stage 2 - Angular Build'
		  sh 'cd $WORKSPACE/angular_todo'
          sh 'npm install'
        }
      }
    }
  stage('Stage 3 - NodeJS Build') {
      steps {
        script {
          echo 'Stage 3 - NodeJS Build'
		  sh 'cd $WORKSPACE/node_todo_api'
		  sh 'npm install'
        }
      }
    }

  stage('Stage 4 - Angular Test') {
      steps {
        script {
          echo 'Stage 4 - Angular Test'
		  sh 'cd $WORKSPACE/angular_todo'
          sh 'npm test'
		  sh 'npm run-script --silent lint> lint.xml'
        }
      }
    }

  stage('Stage 5 - NodeJS Test') {
      steps {
        script {
          echo 'Stage 5 - NodeJS Test'
		sh 'cd $WORKSPACE/node_todo_api'
		sh 'npm test'
		sh 'npm run-script --silent lint> lint.xml'
        }
      }post {
                always {
                    junit '**/test-report.xml'
                }
            }
    }

  stage('Stage 6 - Sonarqube') {
    steps {
	
        container('SonarQubeScanner') {
            withSonarQubeEnv('SonarQube') {
                sh "/opt/scan \
				-Dsonar.projectKey=AncyProject \
				-Dsonar.sources=. \
				-Dsonar.host.url=https://2886795326-9000-host08-fresco.environments.katacoda.com \
				-Dsonar.login=397f810ac0b3d4e57f15a20c6c3b6bd7f541b540"
            }
            timeout(time: 10, unit: 'MINUTES') {
			def qualitygate = waitForQualityGate()
			if (qualitygate.status != "OK") {
				error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
				}
                waitForQualityGate abortPipeline: true
            }
			
			
        }
    }
}
	stage('Stage 7 - Wait') {
      steps {
        script {
          echo 'Stage 7 - tHE end'
		}
      }
    }

  }
}