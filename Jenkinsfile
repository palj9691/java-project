pipeline {
  agent { label 'linux' }
    git url: 'https://github.com/palj9691/java-project.git', branch: 'master'
    stage ("UnitTest") {
      steps {
        sh "ant -f test.xml -v"
        junit "reports/result.xml"
        }
    }
 }
        

