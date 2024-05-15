pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Name of the NodeJS installation in Jenkins
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the Angular project
                bat 'bat 'npm run build -- --configuration production'
            }
        }

        stage('Package') {
            steps {
                script {
                    // Package the build output as a WAR file
                    bat '''
                    mkdir -p dist/war
                    cp -r dist\\angular-jenkins\\* dist\\war\\
                    cd dist/war
                    jar -cvf angular-jenkins.war *
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Define variables
                    def tomcatPath = "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps"
                    def warFile = "dist\\war\\angular-jenkins.war"

                    // Stop Tomcat
                    bat "net stop Tomcat10"

                    // Copy WAR file to Tomcat webapps directory
                    bat "copy ${warFile} ${tomcatPath}\\your-angular-project.war"

                    // Start Tomcat
                    bat "net start Tomcat10"
                }
            }
        }
    }
}
