pipeline {

    agent any
/*
	tools {
        maven "maven3"
    }
*/


    stages{

        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }



        stage ('Docker Build') {
         
            docker.build('springboot_ecr')
        }

        stage ('Docker Push'){
            withDockerRegistry(credentialsId: 'ecr:us-east-1:aws_cred', url: '373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr') {
    docker.image('springboot').push('latest')
        }
        }

    //     stage('Kubernetes Deploy') {
	//   agent { label 'KOPS' }
    //         steps {
    //                 sh "helm upgrade --install --force vproifle-stack helm/vprofilecharts --set appimage=${registry}:${BUILD_NUMBER} --namespace prod"
    //         }
    //     }

    }


}