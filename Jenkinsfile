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
        stage("test"){
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
}
}

     def registry = 'https://mundati.jfrog.io/'
         stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artifact-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "10-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }      


