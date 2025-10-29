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

            echo "📂 Syncing project files..."
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}

            echo "📦 Installing dependencies..."
            sudo npm install

            echo "🏗️ Building Next.js app..."
            sudo npm run build

            echo "🛑 Killing old process on port 3000..."
            sudo fuser -k 3000/tcp || true

            echo "🚀 Starting Next.js app in background..."
            npm run start

            sleep 3
            echo "✅ Deployment completed. App should now be running on port 3000."
        """
    }
}
