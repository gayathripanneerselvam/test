pipeline {
agent any
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

stage ('Build') {
steps {
git "https://github.com/john-smart/game-of-life.git"
sh 'mvn clean package'
}
post {
success {
archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
 junit '**/target/surefire-reports/*.xml' 
}
}
}
  stage ('Deploy'){
  steps {
sshagent(['1f2d204a-0f50-4571-8eac-86c70ebc1d30']) {
    sh "scp -o StrictHostKeyChecking=no target/*.jar admin@13.232.175.7:/opt/tomcat/webapps"
}
	}
	}
}
}
