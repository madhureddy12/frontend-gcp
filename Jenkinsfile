pipeline {

  environment {
    PROJECT = "gcp-lab-240205"
    APP_NAME = "hipster-frontend"
    CLUSTER = "gke-private-test-cluster"
    CLUSTER_ZONE = "asia-southeast1-b"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}"
    JENKINS_CRED = "${PROJECT}"
  }

  agent {
    kubernetes {
      label 'frontend'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: ci-jenkins
  containers:
  - name: golang
    image: golang:1.12-alpine
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('Test') {
      steps {
        container('golang') {
          sh """
            echo "******** currently executing Test stage ********"
          """
        }
      }
    }
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud') {
          sh """
             echo "******** curretly executing Build and Push stage **********"
             PYTHONUNBUFFERED=1 gcloud builds submit -t ${IMAGE_TAG}:${env.BRANCH_NAME}.${env.BUILD_NUMBER} .
             gcloud container images add-tag ${IMAGE_TAG}:${env.BRANCH_NAME}.${env.BUILD_NUMBER} ${IMAGE_TAG}:latest
           """
        }
      }
    }
  } 
}

