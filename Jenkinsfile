pipeline {
    agent { label 'dev' }

    environment {
        JAVA_HOME = "/usr/java/jdk1.8.0_241-amd64"
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
        MAVEN_HOME = "/opt/apache-maven-3.9.9"
        MAVEN_CMD = "${env.MAVEN_HOME}/bin/mvn"
        BITS_TOMCAT_WEBAPPS = "/usr/tomcat/tomcat-8.5.65/webapps"
        
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev-stage',
                    url: 'https://github.com/JavakarBits/ezeebits.git'
            }
        }

        stage('Build WAR') {
            steps {
                dir('busservices') {
                    sh '''
                        echo "Using Java version:"
                        java -version
                        echo "Using Maven version:"
                        ${MAVEN_CMD} -v
                        ${MAVEN_CMD} clean package
                    '''
                }
            }
        }

        stage('Move WAR') {
            steps {
                sh '''
                    mkdir -p /opt/stage/war
                    mv busservices/target/busservices.war /opt/stage/war/
                '''
            }
        }

        stage('Deploy WAR to Tomcats') {
            steps {
                sh '''
                    echo "Stopping both Tomcats..."
                    sudo /etc/init.d/srmBitsTomcat stop || true
                    
                    echo "Sleeping for 5 seconds to allow Tomcat to fully stop..."
                    sleep 5

                    echo "Cleaning old deployments..."
                    rm -rf ${BITS_TOMCAT_WEBAPPS}/busservices
                    rm -f ${BITS_TOMCAT_WEBAPPS}/busservices.war
 

                    echo "Copying WAR files..."
                    cp /opt/stage/war/busservices.war ${BITS_TOMCAT_WEBAPPS}/

                    echo "Starting both Tomcats in background..."
                    sudo nohup /etc/init.d/srmBitsTomcat start 
                    sudo nohup /etc/init.d/cargoStageTomcat start 

                    echo "Deployment triggered."
                '''
            }
        }

        stage('Optional - Wait & Log Check') {
            steps {
                sh '''
                    echo "Waiting 15 seconds for services to boot..."
                    sleep 30
                    echo "Checking catalina logs (tail last 20 lines)..."
                    tail -n 60 /usr/tomcat/tomcat-8.5.65/logs/catalina.out || true
                    tail -n 60 /usr/tomcat/cargotomcat-8.5.51/logs/catalina.out || true
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment completed successfully!"
        }
        failure {
            echo "❌ Build or Deployment failed!"
        }
    }
}
