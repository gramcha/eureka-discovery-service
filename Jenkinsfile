
node {
	def app
	def mavendocker
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }
    stage('Prerequisite') {
    		mavendocker =docker.image('maven:3.3.3')
    		sh 'echo "maven ready"'
    }
    stage('Build') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
         mavendocker.inside {
            sh 'mvn install -DskipTests'
        }
        app = docker.build("eureka-discovery-service")
    }
    stage('Test') {
			mavendocker.inside {
				sh 'mvn test'
			}
			junit 'target/surefire-reports/*.xml'
    }
    stage('Publishing to Dockerhub'){
            echo "--------------------"
            echo ${env.BUILD_NUMBER}
            echo "--------------------"
    		docker.withRegistry([ credentialsId: "${dockerhub}", url: "" ]) {
            app.push("${env.BUILD_NUMBER}")
        }
 	}

}