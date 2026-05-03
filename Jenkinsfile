pipeline {
  agent any

  environment {
    FRONTEND_IMAGE = "mern-frontend:jenkins"
    BACKEND_IMAGE  = "mern-backend:jenkins"
    PORT = "5003"
    MONGO_URI = "mongodb://mongo:27017/devops-course"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/thanaponnnn1/devops-youtube-course-2025', branch: 'main'
      }
    }

    stage('Prepare .env') {
      steps {
        sh '''
          mkdir -p server
          cat > server/.env <<EOF
PORT=$PORT
MONGO_URI=$MONGO_URI
EOF
        '''
      }
    }

    stage('Build Docker Images') {
      steps {
        sh '''
          echo "Clearing old containers..."
          docker compose down || true
          
          echo "Building backend image..."
          docker build -t $BACKEND_IMAGE ./server

          echo "Building frontend image..."
          docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5003/api
        '''
      }
    }

    stage('Run with Docker Compose') {
      steps {
        sh '''
          echo "Starting MERN stack with Docker Compose..."
          docker compose up -d

          echo "Showing running containers..."
          docker ps

          echo "===== Backend Logs ====="
          docker logs backend || true

          echo "===== Frontend Logs ====="
          docker logs frontend || true
        '''
      }
    }
  }
}
