pipeline {
environment {
registry = "seivios/test1"
registryCredential = 'dockerhub_id'
dockerImage = ''
}
agent any
stages {
stage('SonarQube analysis') {
steps{
 withSonarQubeEnv('SonarQube') {
            sh './gradlew sonarqube'
}
}
}
stage('Building our image') {
steps{
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy our image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('Run our image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.run()
}
}
}
}
stage('Cleaning up') {
steps{
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
}
