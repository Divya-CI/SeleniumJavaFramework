pipeline {
	agent any
	parameters {
      string defaultValue: 'chrome', description: 'select a browser', name: 'browser', trim: false
      string defaultValue: 'local', description: 'select a platform', name: 'platform', trim: false
    }

tools
{
	maven 'M3'
}
stages
{
	stage('checkout stage')
	{
	steps{
        echo "checkingout stage"
        echo "${params.browser}"
        deleteDir()
        git branch: 'main', credentialsId: 'Divya_git', url: 'https://github.com/Divya-CI/SeleniumJavaFramework.git'
        }
    }
    stage('Test'){
        steps{
        catchError{
            echo "Test stage"
            bat "mvn test -Dbrowser=${params.browser} -Dplatform=${params.platform} -Dmaven.test.failure.ignore=true"
            }
            echo currentBuild.result
        }

    }
    stage('deploy'){
            steps{
                echo "deploy stage"
            }
    }
}
post{
    always{
        echo "artifact"
        archiveArtifacts artifacts: 'reports/spark.html', followSymlinks: false
        archiveArtifacts artifacts: 'target/surefire-reports/emailable-report.html'
        junit allowEmptyResults: true, skipMarkingBuildUnstable: true, skipPublishingChecks: true, testResults: 'target/surefire-reports/*.xml'
	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'reports/spark.html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    }
}
}
