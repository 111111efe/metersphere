pipeline {
    agent any
    stages {
        stage('maven-build') {
            steps {
                script {
                    sh """
                        mvn clean package -Dmaven.test.skip=true
                    """
                }

            }
        }

        stage('docker-build') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'metersphere-registry', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh """
                            moduleName=metersphere
                            docker login -u $user -p $pass registry.cn-qingdao.aliyuncs.com
                            docker build --rm  --pull -t registry.cn-qingdao.aliyuncs.com/metersphere/\$moduleName:$BRANCH_NAME -t registry.cn-qingdao.aliyuncs.com/metersphere/\$moduleName:${BRANCH_NAME}_b${BUILD_NUMBER} .
                            docker push registry.cn-qingdao.aliyuncs.com/metersphere/\$moduleName:$BRANCH_NAME
                            docker push registry.cn-qingdao.aliyuncs.com/metersphere/\$moduleName:${BRANCH_NAME}_b${BUILD_NUMBER}
                        """
                    }
                }

            }
        }

    }
}
