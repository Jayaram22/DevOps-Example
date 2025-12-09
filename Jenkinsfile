node {
    // Name of Maven tool from Jenkins > Manage Jenkins > Global Tool Configuration
    def mvnHome = tool 'Maven-3.9.11'   // <-- change only if your tool name is different

    // Docker image name
    def appImage = "vickeyyvickey/devopsdemo"
    def dockerImage

    stage('Clone Repo') {
        // If you have your own fork, replace with your repo URL
        git 'https://github.com/vikas4cloud/DevOps-Example.git'
    }

    stage('Build with Maven') {
        sh "'${mvnHome}/bin/mvn' -version"
        sh "'${mvnHome}/bin/mvn' clean package -DskipTests"
    }

    stage('Build Docker Image') {
        // Builds image using Dockerfile in the repo
        dockerImage = docker.build("${appImage}:${env.BUILD_NUMBER}")
        sh "docker images"
    }

    stage('Docker Login') {
        // ⚠ In production use withCredentials + Jenkins credentials
        sh "docker login -u vickeyyvickey -p Hello@123"
    }

    stage('Docker Push') {
        // Push tagged image and 'latest'
        echo "Pushing image: ${appImage}:${env.BUILD_NUMBER}"
        dockerImage.push()
        dockerImage.push("latest")
    }
}
