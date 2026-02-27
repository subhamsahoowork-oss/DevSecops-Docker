pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_new"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }
        }
           stage('Test') {
            steps {
                sh "ls ; mvn test"
            }
            post{
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco (execPattern: 'target/jacoco.exec')
                  }
             }
        }
           stage('Docker Build and Push') {
            steps {
              withDockerRegistry(credentialsId: 'gitlab', url: 'https://gitlab.io/')  {
                sh 'printenv'
                sh 'docker build -t gitlab.io/subhamsahoo.work/subhamsahoo.work-project:""$GIT_COMMIT"" .'
                sh 'docker push gitlab.io/subhamsahoo.work/subhamsahoo.work-project:""$GIT_COMMIT""'
            }
         }
      }
   }   
}
