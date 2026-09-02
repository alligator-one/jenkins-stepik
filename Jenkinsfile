pipeline {
    agent any

    environment {
        DEPLOY_ENV = 'staging'
        // Для удобства объявим переменную, но заполним позже
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    // Получаем имя текущей ветки и сохраняем в env.BRANCH_NAME (переопределяем)
                    env.BRANCH_NAME = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    // Если команда не сработает (например, в detached HEAD), используем запасной вариант:
                    if (env.BRANCH_NAME == 'HEAD') {
                        env.BRANCH_NAME = sh(script: 'git name-rev --name-only HEAD', returnStdout: true).trim()
                    }
                    echo "Detected branch: ${env.BRANCH_NAME}"
                }
            }
        }

        // Часть 1: Условие по ветке
        stage('Build') {
            steps {
                echo "Building application..."
                echo "Current branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying to production environment"
                echo "Branch: main - deployment allowed"
            }
        }

        // Часть 2: Условие по переменной окружения
        stage('Deploy to Staging') {
            when {
                environment name: 'DEPLOY_ENV', value: 'staging'
            }
            steps {
                echo "Deploying to staging environment"
                echo "Environment: ${DEPLOY_ENV}"
            }
        }

        stage('Deploy to Production (by env)') {
            when {
                environment name: 'DEPLOY_ENV', value: 'production'
            }
            steps {
                echo "Deploying to production environment"
                echo "Environment: ${DEPLOY_ENV}"
            }
        }

        // Часть 3: Условие с expression
        stage('Run Tests') {
            when {
                expression { env.BUILD_NUMBER.toInteger() % 2 == 0 }
            }
            steps {
                echo "Running tests for build ${env.BUILD_NUMBER}"
                echo "This is an even-numbered build"
            }
        }

        stage('Skip Tests') {
            when {
                expression { env.BUILD_NUMBER.toInteger() % 2 != 0 }
            }
            steps {
                echo "Skipping tests for build ${env.BUILD_NUMBER}"
                echo "This is an odd-numbered build"
            }
        }

        // Часть 4: Комбинированные условия (исправлено)
        stage('Security Scan') {
            when {
                expression {
                    (env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'develop') &&
                    (env.DEPLOY_ENV == 'staging' || env.DEPLOY_ENV == 'production')
                }
            }
            steps {
                echo "Running security scan"
                echo "Branch: ${env.BRANCH_NAME}, Environment: ${env.DEPLOY_ENV}"
            }
        }

        // Часть 5: Итоговый отчет
        stage('Summary') {
            steps {
                echo "=== Pipeline Execution Summary ==="
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Deploy Environment: ${env.DEPLOY_ENV}"
                echo "All stages completed"
            }
        }
    }
}