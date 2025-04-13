pipeline {
    agent any

    environment {
        JMETER_HOME = '/athena/jmeter/apache-jmeter-5.5/bin'
        JMX_FILE = "${env.WORKSPACE}/SimplePerfMall.jmx"
        RESULT_FILE = "${env.WORKSPACE}/result.jtl"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('📦 打印 Jenkins 工作空间') {
            steps {
                echo "当前 Jenkins 工作空间路径是：${env.WORKSPACE}"
            }
        }

        stage('⏰ 生成动态报告目录变量') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy_MM_dd_HHmmss", TimeZone.getTimeZone('Asia/Shanghai'))
                    env.REPORT_DIR = "${env.WORKSPACE}/report_${timestamp}"
                    echo "报告目录：${env.REPORT_DIR}"
                }
            }
        }

        stage('✅ 检查 JMX 文件') {
            steps {
                script {
                    if (!fileExists("${JMX_FILE}")) {
                        error "❌ 未找到 JMX 测试脚本：${JMX_FILE}"
                    }
                }
            }
        }

        stage('🚀 执行 JMeter 测试') {
            steps {
                sh """
                    ${JMETER_HOME}/jmeter -n \\
                    -t ${JMX_FILE} \\
                    -l ${RESULT_FILE} \\
                    -e -o ${REPORT_DIR}
                """
            }
        }

        stage('📄 输出报告目录') {
            steps {
                echo "JMeter 性能测试报告生成于：${env.REPORT_DIR}"
            }
        }
    }

    post {
        success {
            echo '✅ 构建成功并完成 JMeter 性能测试报告生成！'
        }
        failure {
            echo '❌ 构建失败，请检查日志或配置。'
        }
    }
}
