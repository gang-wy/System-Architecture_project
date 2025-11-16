//Jenkins
def DOCKER_USERNAME = "withyou02" 

pipeline {
    agent any // Jenkins가 실행될 환경

    environment {
        DOCKER_CREDS = credentials('DOCKER_HUB_CREDS')
    }

    stages {
        // 3. GitHub에서 최신 코드를 가져온다
        stage('Checkout Code') {
            steps {
                echo "CI: 1. Checking out code from GitHub..."
                checkout scm
            }
        }

        // 4. "기본 기술"(bat)로 5개 서비스 빌드 & 푸시
        stage('Build & Push All Services') {
            parallel {
                stage('Auth Service') {
                    steps {
                        bat """
                            echo "CI: Building auth_service..."
                            docker build --no-cache -t ${DOCKER_USERNAME}/auth-app:v1 ./auth_service
                            echo "CI: Logging in to Docker Hub..."
                            echo "${DOCKER_CREDS_PSW}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            echo "CI: Pushing auth-app:v1..."
                            docker push ${DOCKER_USERNAME}/auth-app:v1
                        """
                    }
                }
                stage('Product Service') {
                    steps {
                        bat """
                            echo "CI: Building product_service..."
                            docker build --no-cache -t ${DOCKER_USERNAME}/product-app:v1 ./product_service
                            echo "${DOCKER_CREDS_PSW}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            docker push ${DOCKER_USERNAME}/product-app:v1
                        """
                    }
                }
                stage('Order Service') {
                    steps {
                        bat """
                            echo "CI: Building order_service..."
                            docker build --no-cache -t ${DOCKER_USERNAME}/order-app:v1 ./order_service
                            echo "${DOCKER_CREDS_PSW}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            docker push ${DOCKER_USERNAME}/order-app:v1
                        """
                    }
                }
                stage('Inventory Blue') {
                    steps {
                        bat """
                            echo "CI: Building inventory_service_blue..."
                            docker build --no-cache -t ${DOCKER_USERNAME}/inventory-app:v1 ./inventory_service_blue
                            echo "${DOCKER_CREDS_PSW}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            docker push ${DOCKER_USERNAME}/inventory-app:v1
                        """
                    }
                }
                stage('Inventory Green') {
                    steps {
                        bat """
                            echo "CI: Building inventory_service_green..."
                            docker build --no-cache -t ${DOCKER_USERNAME}/inventory-app:v2 ./inventory_service_green
                            echo "${DOCKER_CREDS_PSW}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                            docker push ${DOCKER_USERNAME}/inventory-app:v2
                        """
                    }
                }
            } // end parallel
        } // end stage
    } // end stages
} // end pipeline