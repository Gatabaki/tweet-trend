pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment {
    PATH ="/opt/maven/apache-maven-3.9.3/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------- build started ------"
                sh 'mvn clean deploy'
                echo "-------build completed ------"
            }
        }
        stage("test") {
            steps{
                echo "-------- unit test started --------"
                sh 'mvn surefire-report:report'
                echo "--------- unit test completed ----------"
            }
        }

    stage('SonarQube analysis') {
    environment {
      scannerHome = tool 'sonar-scanner'
    }
    steps{
    withSonarQubeEnv('sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
    }
  } 
stage("Quality Gate"){
    steps {
        script {
  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
}
  

  }  
}

}