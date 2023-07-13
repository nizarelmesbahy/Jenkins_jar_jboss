pipeline {
    agent any
    
    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/nizarelmesbahy/Jenkins_jar_jboss.git'
            }
        }
        
        stage('Build') {
            steps {
                dir('app') {
                    // Use Maven or Gradle to build your war file
                    // For example, using Maven:
                    // bat 'mvn clean package'

                  bat 'ng build --configuration production --base-href=app'
                  
                  
                }
            dir('app/dist/app'){
                  bat 'jar -cvf app.war *'             
                              }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Run a Docker container from the existing JBoss EAP 7 image
                    def container = docker.image('daggerok/jboss-eap-7.3').run('-p 8082:8080 –p 9990:9990 –p 8443:8443')
                    
                    // Copy the war file to the container
                    docker.cp('app/dist/app/app.war', "${container.id}:/home/jboss/jboss-eap-7.3/standalone/deployments/")
                    
                  //${container.id}                    
                    // Restart the JBoss EAP 7 server inside the container
                    docker.exec("${container.id}", 'bat', '-c', '/home/jboss/jboss-eap-7.3/bin/standalone.sh --restart')
                }
            }
        }
    }
}
