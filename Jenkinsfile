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
                }
            }
      }
  
      //stage('SonarQube analysis') {
         //Ws(/var/lib/jenkins)
    // requires SonarQube Scanner 2.4+
        // steps {
        // def scannerHome = tool 'SonarQube Scanner 2.4';
     
        //withSonarQubeEnv('My SonarQube Server') {
     //sh "${scannerHome}/opt/sonar/sonar-scanner"
    //}
 // }
 //}
       stage('Deploy to Tomcat'){
  steps {
  sshagent(['e06a00da-1e5c-492a-b752-f6863ab77dcf']) {
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
