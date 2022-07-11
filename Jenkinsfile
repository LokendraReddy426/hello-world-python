pipeline {
    agent any
	environment {
		PROJECT_ID = 'hashedin-350203'
                CLUSTER_NAME = 'my-demo-cluster'
                LOCATION = 'us-east1-c'
                CREDENTIALS_ID = 'kubernets'
		docker_ID = 'dockerhub'
		imageName = "mypythonapp"
		dockerImage = ''
	}
	
    stages {
	    stage('Scm Checkout') {
		    steps {
			    checkout scm
		    }
	    }
	    stage('docker login') {
           steps {
               script {
		       sh "docker login -u lokey0426 -p ${dockerhub}"
               }
           }
       }
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
			    sh "sed -i 's/tagversion/${env.BUILD_ID}/g' service.yaml"
				sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
				echo "Start deployment of deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
			    echo "Deployment Finished ..."
		    }
	    }
    }
}

