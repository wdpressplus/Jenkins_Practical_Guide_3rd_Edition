pipeline {
    agent any
    stages {
        // Mark the code checkout 'stage'....
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git url: 'https://github.com/wdpressplus/Jenkins_Practical_Guide_3rd_Edition.git'
            }
        }

        // Mark the code build 'stage'....
        stage('Build, Unittest and Static Analysis') {
            steps {
                script {
                    // Global Tool Configuration で Maven 3.5.0 を設定していること。
                    def mvnHome = tool 'Maven 3.5.0'
                    if (env.OS == 'Windows_NT') {
                        bat "${mvnHome}/bin/mvn clean package"
                    } else {
                        sh "${mvnHome}/bin/mvn clean package"
                    }
                }
            }
        }

        // Mark the Report of Unittest and Coverage 'stage'....
        stage('Report of Unittest and Coverage') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                step([$class: 'JacocoPublisher'])
            }
        }
    
        // Mark the Static Analysis 'stage'....
        stage('Report of Static Analysis') {
            steps {
                step([$class: 'CheckStylePublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/target/checkstyle-result.xml', unHealthy: ''])
                step([$class: 'FindBugsPublisher', canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '**/target/findbugsXml.xml', unHealthy: ''])
            }
        }
   }
}
