pipeline {
    agent any
    stages {
        stage('Check Out GIT repo') {
            steps {
                git branch: 'v2.0', url: 'https://github.com/ng-rh/employee-app-v2.git'
                
            }
            
        }
        stage('Login to a dev cluster') {
            steps {
                sh "oc login https://api.cluster-nn9bx.nn9bx.sandbox1323.opentlc.com:6443 --username=opentlc-mgr --password=r3dh4t1! --insecure-skip-tls-verify"
                
            }
            
        }
        stage('Delete Existing Project'){
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                   sh "oc delete microservice"}
            }
        }
        stage('Login to namespace') {
            steps {
                sh "oc new-project microservice"
                
            }
        }
        stage('Create Jenkins Service Account') {
            steps {
                sh "oc create sa jenkins-service-account"
                
            }
        }
        stage('Give access to the dev user') {
            steps {
                sh "oc policy add-role-to-user edit system:serviceaccount:default:jenkins-service-account -n microservice"
                
            }
            
        }
        stage('Deploy v2 in Dev Env') {
            steps {
                sh "oc new-app --name employee java:11~https://github.com/ng-rh/microservice.git"
            }
            
        }
        stage('Expose the service for employee-v2 in Dev Env') {
            steps {
                sh "oc expose service employee -n microservice"
                
            }
            
        }
        stage('Approve or Reject') {
            steps {
                input 'Approve?'
                
            }
        }
    }
}
