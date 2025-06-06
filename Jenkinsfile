pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/meghavathveeresh/Petclinic.git'
            }
        }
          stage('Build') {
            steps {
               bat 'mvn clean install'
            }
        }
        stage('SonarQube Server') {
            steps {
                bat 'mvn install'
              bat '''mvn sonar:sonar\
  -Dsonar.projectKey=Petclinic\
  -Dsonar.projectName='Petclinic'\
  -Dsonar.host.url=http://localhost:9000\
  -Dsonar.token=sqp_b0c08bfd7b4a65660fc731f266aa096bdab5b2df'''
            }
        }
          stage('Generate Artifacts') {
            steps {
               archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
		stage('Deploy to Tomcat Server') {
        steps {
            deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'AMDN', path: '', url: 'http://localhost:8080/')], contextPath: 'Petclinic', war: 'target/*.war'
        }
    }
    }
}
