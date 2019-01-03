pipeline {
   agent {label "master"}
    
    tools {
        maven 'maven-3.5.2'
        jdk 'jdk1.8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

       
      
      stage ('Compile-Package') {
            steps {
                git "https://github.com/gayathripanneerselvam/myapp.git"
              sh 'mvn clean package'
            }
          post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                    junit '**/target/surefire-reports/*.xml' 
                   s3Upload consoleLogLevel: 'INFO', dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'gayathri-jenkins-poc', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-south-1', showDirectlyInBrowser: false, sourceFile: '**/target/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'gayathri-jenkins-poc', userMetadata: []
                }
            }
      }
  
      //stage ('Sonar-Testing') {
        
//environment {
// def scannerhome = tool 'sonar'
 //   }
 //steps {
   //withSonarQubeEnv ('sonar') 
//{
//sh "${scannerhome}/bin/sonar -D sonar.projectKey=app -D sonar.projectName=app -D sonar.projectVersion=1.0  -D sonar.web.host=sonar -D sonar.web.port=9000 -D sonar.sources=/var/lib/jenkins/workspace/temp-pipeline/app package/src -D sonar.url=http://13.233.4.162:9000/sonar"
  // }
//}
  //  } 
       stage('Deploy to Tomcat'){
  steps {
  sshagent(['9545974a-02f5-4ad2-b0c8-a9b4a3d340e7']) {
    sh "scp -o StrictHostKeyChecking=no target/*.war admin@13.232.175.7:/opt/tomcat/webapps"
    
    }
    }
    }
  stage('Run JMeter Test') {
        steps {
        
                sh '/root/apache-jmeter-5.0/bin/jmeter.sh -n -t /root/apache-jmeter-5.0/bin/ForEachTest2.jmx -l $WORKSPACE/build-result.jtl'
            }
        }
  
       stage ('Email Notification'){
          steps{
      emailext (
    subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
    body: """<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",      
    to: "gayathri.electron@gmail.com",
    from: "gayathri.electron@gmail.com"
)
          }
       }
      }
   }
