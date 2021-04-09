env.DOCKER_REGISTRY = 'miqbalnawawi'
env.DOCKER_IMAGE_NAME = 'backend'
node('master') {
	stage('HelloWorld') {
      echo 'Hello World'
    }
    stage('Git Pull from Github') {
        sh 'git clone https://github.com/miqbalnawawi/CICD-MERN.git'
    }
    stage('Build Docker Image') {
        sh "cd CICD-MERN/mern-todo-app && docker build --build-arg APP_NAME=backend -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Dockerhub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('DeployTo Kubernetes Cluster') {
        sh'''cd CICD-MERN/mern-todo-app && sed -i "15d" backend-deployment.yml'''
        sh'''cd CICD-MERN/mern-todo-app && sed -i "14 a \'\\'          image: $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}" backend-deployment.yml && sed -i "s/''//" backend-deployment.yml'''
        sh 'cd CICD-MERN/mern-todo-app && kubectl apply -f backend-deployment.yml'
   }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage("Clean Workspace"){
     cleanWs() 
        
    }
}
