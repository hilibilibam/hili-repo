pipeline {

  agent any
  customImage = ""

  stage("parameter") {
        properties([parameters([choice(choices: ['enabled', 'disabled'], name: 'deploy')])])
    }
    stage('choice') {
        def deploy = params.deploy
        if (deploy == 'enabled') {
            echo deploy
            sh "sudo docker run -t -d --rm training/webapp:latest"
        } else {
            echo "no other actions will be performed"
        }
    }
}
  
  stage("create dockerfile") {

    sh """
    tee Dockerfile <<-'EOF'
    FROM ubuntu:latest
    RUN touch file-01.txt
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
stage("test") {
    slackColor = "good"
    end = "success"
    try {
      echo "hello world"
    } catch (Exception e) {
      slackColor = "danger"
      end = "failure"
      currentBuild.result = "FAILURE"
    } finally {
        slackSend color: slackColor, message: "Build finished with ${end}: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
    }
}
}
