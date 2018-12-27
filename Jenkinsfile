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
git "https://github.com/kliakos/sparkjava-war-example.git"
sh 'mvn clean package'
}
post {
success {
junit '**/target/*.xml' 
}
}
}
}
}
