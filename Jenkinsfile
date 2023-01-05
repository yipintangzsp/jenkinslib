#!groovy
@Library('jenkinslib')_


def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"

pipeline{
    agent{
        node {
            label "master"
            customworkspace "${workspace}"
        }
    }
    
    options{
        timestamp()
        skipDefaultCheckout()
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
    }
    
    stages {
        stage("GetCode"){
            when {envoronment name: 'test', value: 'abcd'}
            step{
                timeout(time:5, unit:"MINUTES"){
                    script{
                        println('获取代码')
                        println('${test}')
                        
                        input id: 'Test', message: '我们是否要继续？', ok : '是，继续吧！', parameters [choices: ['a','b'] ]
                    }
                }

            }
        }
        
        stage("01"){
            failFast true
            parallel {
                stage("Build"){
                    steps{
                        timeout(time:20, unit:"MINUTES"){
                            script{
                                println('应用打包')
                                
                            }
                        }
                    }
                }
                
                stage("CodeScan"){
                    steps{
                        timeout(time:30, unit:"MINUTES"){
                            script{
                                print("代码扫描")
                            }
                        }
                    }
                }
            }
        }
    }
    
    
    post{
        always {
            script{
                println("always")
            }
        }
        
        success {
            script{
                currentBuild.description = "\n 构建成功 !"
            }
        }
        
        
        failure {
            script{
                currentBuild.description = "\n 构建失败 !"
            }
        }
        
        aborted{
            script{
                currentBuild.description = "\n 构建取消!"
            }
        }
    }
}
