pipeline {
    agent any

    environment {
        DEPLOY_ENV = 'production'
        // CURRENT_BRANCH будет заполнена позже в стейдже Checkout
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    // Получаем имя ветки и сохраняем в переменную окружения
                    env.CURRENT_BRANCH = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    // Если ветка не определилась (например, сборка тега), используем fallback
                    if (env.CURRENT_BRANCH == 'HEAD') {
                        env.CURRENT_BRANCH = sh(script: 'git name-rev --name-only HEAD', returnStdout: true).trim()
                    }
                    echo "Detected branch: ${env.CURRENT_BRANCH}"
                }
            }
        }

        // Часть 1: Условие по ветке (используем expression)
        stage('Build') {
            steps {
                echo "Building application..."
                echo "Current branch: ${env.CURRENT_BRANCH}"
            }
        }

        stage('Deploy to Production') {
            when {
                expression { env.CURRENT_BRANCH == 'main' }
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

        // Часть 3: Условие с expression (чёт/нечет)
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

        // Часть 4: Комбинированные условия (через expression)
        stage('Security Scan') {
            when {
                expression {
                    (env.CURRENT_BRANCH == 'main' || env.CURRENT_BRANCH == 'develop') &&
                    (env.DEPLOY_ENV == 'staging' || env.DEPLOY_ENV == 'production')
                }
            }
            steps {
                echo "Running security scan"
                echo "Branch: ${env.CURRENT_BRANCH}, Environment: ${DEPLOY_ENV}"
            }
        }

        // Часть 5: Итоговый отчет
        stage('Summary') {
            steps {
                echo "=== Pipeline Execution Summary ==="
                echo "Branch: ${env.CURRENT_BRANCH}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Deploy Environment: ${DEPLOY_ENV}"
                echo "All stages completed"
            }
        }
    }
}