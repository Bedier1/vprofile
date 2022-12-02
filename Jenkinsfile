pipeline {

    agent any
    environment {
        DOCKER_REPO = 'mohamedbedier/vprofile_app'
        KUBECONFIG = credentials('minikube')
    }
    /*
    stages {
            stage ("Fetch code") {
            steps {
                git  branch: 'main', url: 'https://github.com/Bedier1/vprofile'
            }
        }
*/

        stage("build-image") {
            steps {
                script {
                        sh "docker build  -f ./docker/app/Dockerfile -t  ${DOCKER_REPO}:${env.BUILD_NUMBER} . "
                      
                        
                    }
                }   
            }
        stage("pushing_image") {
            steps {
                script { 
                    echo "pushing-image"
                    withCredentials( [ usernamePassword(credentialsId: 'docker-repo' , passwordVariable: 'PASS' , usernameVariable: 'USER' )] ) {
 
                       sh """
                         docker login -u  $USER -p $PASS 
                        docker push ${DOCKER_REPO}:${env.BUILD_NUMBER}
                        """
                                         
                        }
                }
            }
        }
        stage("deploy to kubernetes") {
            steps {
                script {
                   withKubeConfig([credentialsId: 'minikube', serverUrl: 'https://192.168.49.2:8443']) {
                         sh """
                         
                         kubectl apply -f ./kubernetes/
                         
                         
                         """
                    }
                }   
            }
  
       
    
    }
}
}