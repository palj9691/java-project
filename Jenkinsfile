properties([pipelineTriggers([githubPush()])])
node('linux') {
    git url: 'https://github.com/palj9691/java-project.git', branch: 'master'
    stage ("UnitTest") {
      steps {
        sh "ant -f test.xml -v"
        junit "reports/result.xml"
        }
    }
    stage ("Build") {
      steps {
        sh "ant -f build.xml -v"
      }
      post {
        success {
          archiveArtifacts artifacts: "$WORKSPACE/target/*.jar"
        }
      }
    }
    stage ("Deploy") {
      steps {
        sh "if ![ -d 'arn::aws:s3://palj9691-assignment-9'], then 'aws mb arn::aws:s3://palj9691-assignment-9'; fi"
        sh ("aws s3 cp $WORKSPACE/target/rectangle-${env.BUILD_NUMBER}.jar s3://palj9691-assignment-9/${env.BRANCH_NAME}/ --recursive --exclude '*' --include '*.jar'")
      }
    }
    stage ("Report") {
        steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                accessKeyVariable: 'AKIAISEWYXHPVE5ZN26A',
                              secretKeyVaraible: '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']]) {
                               
                    sh "aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins"
            }
        }
    }
            
 }
        

