node {
    stage('checkout') {
        checkout scm
    }

    stage('build') {
        def flaskImage = docker.build("flask-app:${env.BUILD_ID}", "--target builder .")
    }

    stage('test') {
        flaskImage.inside {
            sh "nohup python3 app.py &"

            sleep 5

            sh "curl --fail http://localhost:8000 || (echo 'Erreur: l\'application ne répond pas' && exit 1)"
        }
    }


    stage('Publish') {
        docker.withRegistry('https://registry.example.com', 'credentials-id') {
            flaskImage.push()
            flaskImage.push('latest')
        }
    }
}
