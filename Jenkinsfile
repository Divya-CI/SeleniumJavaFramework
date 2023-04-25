pipeline {
	agent any
	parameters {
      string defaultValue: 'chrome', description: 'select a browser', name: 'browser', trim: false
      string defaultValue: 'local', description: 'select a platform', name: 'platform', trim: false
    }

tools
{
	maven 'maven_home'
}
stages
{
	stage('checkout stage')
	{
	steps{
        echo "checkingout stage"
        echo "${params.browser}"
        deleteDir()
        git branch: 'main', credentialsId: 'c5aef89d-c4bf-4c43-8aa6-7def9b90e32c', url: 'https://github.com/sunilpatro1985/SeleniumJavaFramework.git'
        }
    }
    stage('Test'){
        steps{
        catchError{
            echo "Test stage"
            sh "mvn test -Dbrowser=${params.browser} -Dplatform=${params.platform} -Dmaven.test.failure.ignore=true"
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
    }
}
}
