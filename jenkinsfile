node {
  stage('Configure') {
        env.PATH = "${tool 'maven'}/bin:${env.PATH}"
        version = '1.0.' + env.BUILD_NUMBER
        currentBuild.displayName = version

        properties([
                buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10')),
                [$class: 'GithubProjectProperty', displayName: '', projectUrlStr: 'https://github.com/vicky1236/jenkins-cicd-sample'],
                pipelineTriggers([[$class: 'GitHubPushTrigger']])
            ])
 }

 stage('SCM checkout') {
   git 'https://github.com/vicky1236/jenkins-cicd-sample'
 }

 stage('Compile ') {
  sh "mvn clean install"
  junit allowEmptyResults: true, testResults: '**/target/**/TEST*.xml'
 }

 stage('Build Docker image ') {
  sh "docker build -t bharathbg/sample-java-app:${BUILD_NUMBER} ."

 }

 stage('Push to DockerHub') {
   withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerPWD')]) {
     sh "docker login -u bharathbg -p ${dockerPWD}"
   }
   sh "docker push bharathbg/sample-java-app:${BUILD_NUMBER}"
   sh "docker logout"

 }



}
