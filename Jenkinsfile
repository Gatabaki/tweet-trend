pipeline {
    agent {
        node {
            label 'slave-build'
        }
    }
environment {
    PATH ="/opt/maven/apache-maven-3.9.4/bin:$PATH"
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
        stage("test"){
            steps{
                echo "-------- unit test started --------"
                sh 'mvn surefire-report:report'
                echo "--------- unit test completed ----------"
            }
        }

    }
}