pipeline {
  agent {
    docker {
      image 'python:3.11.5-alpine3.18'
      args 'sh \'python -m py_compile sources/add2vals.py sources/calc.py\''
    }

  }
  stages {
    stage('Build') {
      agent {
        docker {
          image 'python:3.11.5-alpine3.18'
        }

      }
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'qnib/pytest'
        }

      }
      post {
        always {
          junit 'test-reports/results.xml'
        }

      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
    }

    stage('Deliver') {
      agent {
        docker {
          image 'cdrx/pyinstaller-linux:python2'
        }

      }
      post {
        success {
          archiveArtifacts 'dist/add2vals'
        }

      }
      steps {
        sh 'pyinstaller --onefile sources/add2vals.py'
      }
    }

  }
}