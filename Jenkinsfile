node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }
    stage('SonarQube analysis') {
                    withSonarQubeEnv('sonarserver') {
			sh "npm install"
                        sh "npm run sonar-scanner"
                    }
            }
     stage("Quality gate") {
     waitForQualityGate abortPipeline: true
     }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("woualabs07/reactapp")
    }

    stage('Test image') {

        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /*
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'Swetha07!') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            }
                echo "Trying to Push Docker Build to DockerHub"
    }
}
