pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'GENERATE_REPORT',
            defaultValue: false,
            description: '是否生成 JMeter HTML 报告（建议关闭，降低内存占用）'
        )
    }

    environment {
        JMETER_HOME = '/athena/jmeter/apache-jmeter-5.5/bin'
        JMX_FILE = "${env.WORKSPACE}/SimplePerfMall.jmx"
        RESULT_FILE = "${env.WORKSPACE}/result.jtl"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('打印工作空间路径') {
            steps {
                echo "当前 Jenkins 工作空间路径：${env.WORKSPACE}"
            }
        }

        stage('生成报告目录变量') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy_MM_dd_HHmmss", TimeZone.getTimeZone('Asia/Shanghai'))
                    env.REPORT_DIR = "${env.WORKSPACE}/report_${timestamp}"
                }
            }
        }

        stage('检查 JMX 文件') {
            steps {
                script {
                    if (!fileExists("${JMX_FILE}")) {
                        error "❌ 未找到 JMX 测试脚本：${JMX_FILE}"
                    }
                }
            }
        }

        stage('执行 JMeter 测试（极限低内存）') {
            steps {
                sh """
                    export JVM_ARGS='-Xms64m -Xmx128m'
                    ${JMETER_HOME}/jmeter -n -t ${JMX_FILE} -l ${RESULT_FILE}
                """
            }
        }

        stage('可选生成 HTML 报告') {
            when {
                expression { return params.GENERATE_REPORT }
            }
            steps {
                sh """
                    export JVM_ARGS='-Xms128m -Xmx256m'
                    ${JMETER_HOME}/jmeter -g ${RESULT_FILE} -o ${REPORT_DIR}
                """
                echo "📊 HTML 报告生成成功：${env.REPORT_DIR}"
            }
        }
    }

    post {
        success {
            echo '✅ 构建成功'
        }
        failure {
            echo '❌ 构建失败，请检查是否为内存不足或 JMX 脚本错误。'
        }
    }
}