node {
	// declaring global mvnHome variable
    def mvnHome
    stage('Preparation') {
        // Get some code from a GitHub repository
        git branch: 'main', credentialsId: 'EcentricGithub', url: 'https://github.com/poojan007/SpringMVC.git'
        // initializing mvnHome variable with the current maven instance
		mvnHome = tool 'MAVEN-3'
    }
    stage('Build') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Deploy') {
		// deploying artifact into remote tomcat9.x server
        deploy adapters: [tomcat9(credentialsId: '8821ac7f-5af0-400a-953f-d1cf12677812', path: '', url: 'http://172.30.3.101:8180/')], contextPath: 'SpringMVC', onFailure: false, war: '**/*.war'
    }
	post {
		cleanup {
			// clean up workspace
			deleteDir()
			// clean up tmp directory
			dir("${workspace}@tmp"){
				deleteDir()
			}
			// clean up script directory
            dir("${workspace}@script") {
                deleteDir()
            }
		}
	}
}
