node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace...'
        deleteDir()
    }

    stage('Clone Repository') {
        echo 'Cloning repository...'
        git branch: 'main', url: 'https://github.com/KahafSameer/CICD_pipeline'
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2 instance...'
        sh """
            # Ensure target directory exists
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Copy files from Jenkins workspace to EC2 directory
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}/

            cd ${appDir}

            # Install dependencies
            npm install --legacy-peer-deps

            # Build the Next.js project
            npm run build

            # Stop any process using port 3000
            sudo fuser -k 3000/tcp || true

            # Start the app with nohup (in background)
            nohup npm start > app.log 2>&1 &
        """
    }
}
