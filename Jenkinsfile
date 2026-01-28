
pipeline {
    agent any
     tools{
         jdk 'Java 17'
         maven 'Maven'
    }
    stages {
        stage('Checkout Code') {
            steps {
               echo "Pulling from GITHUB repository"
               git branch: 'main', credentialsId: 'mygitacc', url: 'https://github.com/masterdoer/dockerimgjenkins.git'
            }
        }
         stage('Test the Project') {
            steps {
               echo "Test my JAVA project"
               bat 'mvn clean test' 
            }
              post {
                  always {
                         junit '**/target/surefire-reports/*.xml'
                         echo 'Test Run succeeded!'          
					}
				}
		}
        stage('Build Project') {
            steps {
               echo "Building my JAVA project"
               bat 'mvn clean package -DskipTests' 
            }
        }
        stage(' Build the Docker Image') {
            steps {
               echo "Build the Docker Image for mvn project"
               bat 'docker build -t mvnproj:1.0 .'
            }
        }
         stage('Push Docker Image to DockerHub') {
            steps {
               echo "Push Docker Image to DockerHub for mvn project"
                 withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'DOCKER_PASS')]) {
                         bat '''
   	        echo %DOCKER_PASS% | docker login -u pratheepp3 --password-stdin
                         docker tag mvnproj:1.0 pratheepp3/mymvnproj:latest
                         docker push pratheepp3/mymvnproj:latest
                         '''
                  }
            }
        }
       
        stage('Deploy the project using Container') {
            steps {
                echo "Running Java Application"
            }
        }
    }

    post {
        success {
            echo 'I succeeded!'
           
        }
        failure {
            echo 'Failed........'
        }
    }
}
