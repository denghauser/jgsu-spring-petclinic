pipeline {

    agent any

    triggers{
        pollSCM('* * * * *') 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'DIETMAR_GITHUB', url: 'https://github.com/denghauser/jgsu-spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }

        }
    }
    post {
        success {
            archiveArtifacts 'target/*.jar'
        }
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
        changed {
            emailext subject: "Job \'${JOB_NAME}\' (build ${BUILD_NUMBER}) ${currentBuild.result}",
                body: "Please go to ${BUILD_URL} and verify the build", 
                attachLog: true, 
                compressLog: true, 
                to: "test@jenkins",
                recipientProviders: [upstreamDevelopers(), requestor()]
        }
    }
}
