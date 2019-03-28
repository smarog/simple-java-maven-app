pipeline{
        agent {
                docker {
                        image 'maven:3-alpine'
                        args '-v /root/.m2:/root/.m2'
                }
        }
        
        stages {
                stage('Build') {
                        steps {
                                sh 'mvn -B -DskipTests clean package'
                        }
                }
     
                stage ('Test') {
                        steps{
                                sh 'mvn test'
                        }
                }
                
                stage('Deliver to developers') { 
                        when{
                                branch 'master'
                        }
                        steps {
                                sh './jenkins/scripts/deliver.sh' 
                        }
                }
                
                stage('Deliver to clients') { 
                        when{
                                branch 'production'
                        }
                        steps {
                                sh './jenkins/scripts/deliver.sh' 
                        }
                }
        }
        
        post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
