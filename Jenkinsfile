node {
    def mavenHome = tool name: 'maven3.8.1'
    stage('SCM Clone') {
       git credentialsId: 'Git-Credentials', url: 'https://github.com/femi-john/spring-boot-docker'
    }
    stage("MavenBuild") {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage("QualityReport") {
       // sh "$(mavenHome)/bin/mvn sonar:sonar"
    }
    stage("ArtifactUpload") {
        // sh "${mavenHome}/bin/mvn deploy"
    }
    stage("BuildDockerimage") {
        sh "docker build -t femijohn/myapp:2 ."
    }
    stage("PushImageToRegistry") {
        withCredentials([string(credentialsId: 'DockerHubCredentials', variable: 'DockerHubCredentials')]) {
            sh "docker login -u femijohn -p ${DockerHubCredentials}"
}
        sh "docker push femijohn/myapp:2"
    }

    stage("RemoveImagefromDockerHub") {
      // sh 'docker rmi $(docker images -q)'
    }
    stage("DeployToK8s") {
        sh "kubectl apply -f springapp.yml"
    }
}
