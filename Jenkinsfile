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

        stage('🚀 执行 JMeter 测试（低内存优化）') {
            steps {
                sh """
                    export JVM_ARGS='-Xms128m -Xmx384m'
                    ${JMETER_HOME}/jmeter -n \\
                    -t ${JMX_FILE} \\
                    -l ${RESULT_FILE}
                """
            }
        }

        stage('📄 提示后续生成报告') {
            steps {
                echo "✅ 测试结果保存在：${env.RESULT_FILE}"
                echo "🔧 建议后续通过如下命令手动生成 HTML 报告："
                echo "${JMETER_HOME}/jmeter -g ${env.RESULT_FILE} -o ${env.REPORT_DIR}"
            }
        }
    }

    post {
        success {
            echo '✅ 构建成功（低内存优化）！请手动生成报告或集成另一个报告任务。'
        }
        failure {
            echo '❌ 构建失败，请检查日志或配置。'
        }
    }
}
