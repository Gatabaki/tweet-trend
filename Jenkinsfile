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
                sh 'mvn clean deploy'
            }
        }

        
    }
}
