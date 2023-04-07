
{

  customImage = ""

  stage("create dockerfile") {

    sh """

    tee Dockerfile <<-'EOF'

    FROM ubuntu:latest

    RUN touch file-02.txt

EOF

"""
  
}
  stage("build docker") {

  customImage =
    docker.build("hilibili/jenkins-ubuntu:latest")

}

  stage("verify dockers") {

    sh "docker images"
}
  stage('Push image') {
    withDockerRegistry(
        credentialsId: 'dockerhub.hilibili', 
        url: 'https://index.docker.io/v1/')
  {
    customImage.push()
    }
}    
}

