pipeline {
environment {
imagename = "theironhidex/hello-world-aws"
registryCredential = 'dockerHubAccount'
dockerImage = ''
}
agent any
stages {
stage('Cloning Git') {
steps {
git([url: 'https://github.com/TheIronhidex/devops-repo-aws.git', branch: 'main', credentialsId: 'gitHubAccount'])
}
}
stage('Building image') {
steps{
script {
dockerImage = docker.build imagename
}
}
}
stage('Deploy Image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push("latest")
dockerImage.push("$BUILD_NUMBER")
}
}
}
}
stage('Deploying image in EC2 instance') {
steps {
sshagent(credentials: ['aws']) {
sh "ssh -vvv -o StrictHostKeyChecking=no -T ubuntu@ec2-34-201-250-94.compute-1.amazonaws.com './bash_script.sh'"
}
}
}
}
}
