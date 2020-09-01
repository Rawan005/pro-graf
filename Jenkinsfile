pipeline {
      agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kubectl
    image: bryandollery/terraform-packer-aws-alpine
    command:
    - cat
    tty: true
  - name: helm
    image: alpine/helm
    command:
    - cat
    tty: true
"""
    }
  }
  environment {
    TOKEN=credentials('10f710e0-c58c-41a6-88fc-6bd58439fdeb')
  }

    stages {
      stage("Deploy") {
          steps {
              container('kubectl') {
                  sh '''
		      kubectl --token=$TOKEN  create namespace monitor
                  '''
              }
          }
      }
        stage('helm-deploy') {
	  steps {
                  container('helm') {
                      sh '''
  		helm repo add stable https://kubernetes-charts.storage.googleapis.com
                helm repo update
		helm install prometheus-operator stable/prometheus-operator --namespace monitor --set grafana.service.type=LoadBalancer
		    '''
                    }
            }
        }
    }
}
