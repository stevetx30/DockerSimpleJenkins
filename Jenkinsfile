node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
        echo "Check out Completed"
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("stevetx30/helloworld")
    }

    stage('Test image') {
        /* Ideally, we will run a test framework against our image.
         * */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'abe7b1d1-8c98-4141-bf9b-03bba1da8027') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}