pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
        jdk 'JAVA'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                bat 'npm install'
                
                // Fix vulnerabilities
                bat 'npm audit fix || exit 0'  // Ensure the build does not fail if some vulnerabilities can't be fixed automatically
            }
        }

        stage('Build') {
            steps {
                // Build the Angular project
                bat 'npm run build -- --configuration production'
            }
        }

        stage('Verify Build Output') {
            steps {
                // List contents of dist directory for debugging
                bat 'dir dist'
                bat 'dir dist\\angular-jenkin'
            }
        }

        stage('Package') {
            steps {
                script {
                    // Package the build output as a WAR file using PowerShell
                    powershell '''
                    $distPath = "dist\\angular-jenkin"
                    $warPath = "dist\\war"
                    
                    # Ensure the war directory exists
                    if (-Not (Test-Path -Path $warPath)) {
                        New-Item -ItemType Directory -Path $warPath
                    }
                    
                    # Copy files from dist\\angular-jenkin to dist\\war
                    Copy-Item -Path "$distPath\\*" -Destination $warPath -Recurse -Force
                    
                    # Change to the war directory
                    Set-Location -Path $warPath
                    
                    # Create the WAR file
                    & jar -cvf angular-jenkins.war *
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

        stage('Verify JDK Installation') {
            steps {
                // Verify if jar command is available
                bat 'jar'
            }
        }
    }
}
