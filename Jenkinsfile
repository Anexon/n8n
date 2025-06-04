pipeline {
  agent any

  environment {
    ENV_SOURCE = '/var/jenkins_envs/n8n.tjuarezruben.com/.env'
    DEPLOY_PATH = '/var/www/n8n.tjuarezruben.com'
    DOCKER_IMAGE = 'n8n'
    ENV_TARGET = '.env'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Prepare .env') {
      steps {
        sh "cp ${ENV_SOURCE} ${ENV_TARGET}"
      }
    }

    stage('Deploy with Compose') {
      steps {
        sh '''
        export $(grep -v '^#' .env | xargs)
        docker-compose down          
        NODE_OPTIONS="--max-old-space-size=2048" docker-compose build --no-cache
        docker-compose up -d --build
        '''
      }
    }
  }
}