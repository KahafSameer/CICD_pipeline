node {
    def appDir = 'var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace...'
        deleteDir()
    }

    stage('Clone Repository') {
        echo 'Cloning repository...'
        git(
            branch: 'main',
            url: 'https://github.com/KahafSameer/CICD_pipeline'

        )
        stage('Deploy to EC2') {
            echo 'Deploying to EC2 instance...'
            sh """
               sudo mkdir -p ${appDir}
               sudo chown -R 
               jenkins:jenkins ${appDir}

               rsync -av --delete --exclude='.git' --exclude='node_modules' .
               /${appDir}/
            """




}