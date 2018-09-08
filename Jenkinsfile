#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('clean') {
        sh "chmod +x gradlew"
        sh "./gradlew clean --no-daemon"
    }


    stage('backend tests') {
        try {
            sh "./gradlew test -PnodeInstall --no-daemon"
        } catch(err) {
            throw err
        } finally {
            junit '**/build/**/TEST-*.xml'
        }
    }

    // stage('frontend tests') {
    //     try {
    //     } catch(err) {
    //         throw err
    //     } finally {
    //         junit '**/build/test-results/jest/TESTS-*.xml'
    //     }
    // }

    // stage('packaging') {
    //     sh "./gradlew bootWar -x test -Pprod -PnodeInstall --no-daemon"
    //     archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
    // }

    // stage('quality analysis') {
    //     withSonarQubeEnv('sonar') {
    //         sh "./gradlew sonarqube --no-daemon"
    //     }
    // }

    // def dockerImage
    // stage('build docker') {
    //     sh "cp -R src/main/docker build/"
    //     sh "cp build/libs/*.war build/docker/"
    //     dockerImage = docker.build('magfin/demo', 'build/docker')
    // }

    // stage('publish docker') {
    //     docker.withRegistry('https://registry.cn-hangzhou.aliyuncs.com', 'docker-login') {
    //         dockerImage.push 'latest'
    //     }
    // }

    post {
        always {
            script {
                // workaround. Seems to be a bug!?
                currentBuild.result = currentBuild.currentResult
            }
            step([
                $class: 'PhabricatorNotifier',
                commentOnSuccess: true,
                commentWithConsoleLinkOnFailure: true,
            ])
        }
    }
}
