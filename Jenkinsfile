pipeline {
    agent none // (1)
    stages {
        stage('Build') { // (2)
            agent {
                docker {
                    image 'python:2-alpine' // (3)
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' // (4)
                stash(name: 'compiled-results', includes: 'sources/*.py*') // (5)
            }
        }
	stage('Test') {
	    agent {
		docker {
		    image 'qnib/pytest'
		}
	    }
	    steps {
		sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
	    }
	    post {
		always {
		    junit 'test-reports/results.xml'
		}
	    }
	}
    }
}
