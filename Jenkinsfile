// Jenkinsfile
def DOCKER_USERNAME = "withyou02" 

pipeline {
    agent any // Jenkins가 실행될 환경

    environment {
        // 2단계에서 Jenkins에 저장한 Docker Hub 자격 증명 ID
        DOCKER_CREDS = credentials('DOCKER_HUB_CREDS')
    }

    stages {
        // 1. GitHub에서 최신 코드를 가져온다
        stage('Checkout Code') {
            steps {
                echo "CI: 1. Checking out code from GitHub..."
                checkout scm
            }
        }

        // 2. Auth 서비스만 CI 테스트 (간단한 예시)
        stage('Build & Push Auth Service') {
            steps {
                script {
                    echo "CI: 2. Building auth-service..."
                    
                    // 3. "손으로" 쳤던 'docker build'를 실행한다.
                    //    [중요] 이미지 태그가 'auth-app:v1'이 아닌,
                    //    Docker Hub ID가 포함된 'my-docker-id/auth-app:v1'이 되어야 함.
                    def img = docker.build("${DOCKER_USERNAME}/auth-app:v1", "./auth-service")
                    
                    // 4. Jenkins의 자격 증명으로 Docker Hub에 로그인
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDS) {
                        
                        // 5. "공용 창고"(Docker Hub)로 이미지를 push한다
                        echo "CI: 3. Pushing image to Docker Hub..."
                        img.push()
                    }
                    echo "CI: 4. Build complete! auth-app:v1 is pushed."
                }
            }
        }
    }
}