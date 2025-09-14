pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-amazon-corretto.x86_64"
        MAVEN_HOME = "/usr/share/maven"
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${PATH}"
        MAVEN_OPTS = "-Dmaven.repo.local=${WORKSPACE}/.m2/repository"
    }
     environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-amazon-corretto.x86_64"
        MAVEN_HOME = "/usr/share/maven"
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${PATH}"
        MAVEN_OPTS = "-Dmaven.repo.local=${WORKSPACE}/.m2/repository"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/venkatesanEzee/jenkins.git'
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                    echo "=== Java Version ==="
                    java -version
                    echo "=== Maven Version ==="
                    mvn -v
                '''
            }
        }

        stage('Build WAR') {
            steps {
                dir('sparkjava-war-example') { // change this to your actual folder containing pom.xml
                    sh '''
                        echo "=== Building WAR ==="
                        mvn clean package --add-opens java.base/java.util=ALL-UNNAMED
                    '''
                }
            }
        }
    }
}
