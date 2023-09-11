pipeline {
    agent any
    tools {
        maven 'MAVEN3'
    }
    environment {
        registry = '729590520513.dkr.ecr.us-west-2.amazonaws.com/hellodatarepo'
        registryCredential = 'awscreds'
        dockerimage = ''
    }
    stages {
        stage("Checkout the project") {
            steps {
                git branch: 'docker', url: 'https://github.com/mandelasasidhar/vprofile-project.git'
            }
        }

        stage("Build the package") {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('CODE ANALYSIS WITH CHECKSTYLE') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }

        stage('JUnit Tests') {
            steps {
                script {
                    def mvnHome = tool name: 'MAVEN3', type: 'hudson.tasks.Maven$MavenInstallation'
                    def junitReportPath = 'target/surefire-reports/**/*.xml'

                    // Run JUnit tests
                    sh "${mvnHome}/bin/mvn test"

                    // Archive JUnit test reports
                    junit allowEmptyResults: true, testResults: junitReportPath
                }
            }
        }
  stage('Integrate Jenkins with EKS Cluster and Deploy') {
                steps {
               withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s-jenkins', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
               sh 'kubectl apply -f workloads.yaml'
}
                }
            }
        
    }
}
