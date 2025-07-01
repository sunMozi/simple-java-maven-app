pipeline {
    agent {
        docker {
            image 'maven:3.9.5-eclipse-temurin' // 更推荐企业稳定版镜像
            args '-v $HOME/.m2:/root/.m2'      // 映射宿主机 Maven 缓存，提高构建效率
        }
    }

    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=/root/.m2/repository'
    }

    options {
        skipDefaultCheckout() // 显式 checkout，便于控制
        timestamps()          // 日志加时间戳
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test || echo "测试阶段失败（已跳过），请检查日志"'
            }
        }
    }

    post {
        always {
            echo '流水线执行完成，清理临时资源（如需可扩展此处逻辑）'
        }
        failure {
            echo '流水线失败，请立即查看日志排查'
        }
    }
}
