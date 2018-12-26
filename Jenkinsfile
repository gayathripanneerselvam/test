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
}
}
