 pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                dir('/Users/nhi.ly/Documents/Core/RepoKatalon/ci-samples-jenkin'){
                    sh 'docker run -t --rm -v "$(pwd)":/tmp/project katalonstudio/katalon katalonc.sh -projectPath=/tmp/project -browserType="Chrome" -retry=0 -statusDelay=15 -testSuitePath="Test Suites/TS_RegressionTest" -apiKey=7024f183-ba93-40a5-bd94-426aa13f9446'
                }
            }
        }
        stage('Upload to TestOps & Xray') {
     		 steps {
		        dir('/Users/nhi.ly/Documents/Core/RepoKatalon/ci-samples-jenkin') {
		          sh '''
		            docker run -t --rm \
		              -v "$(pwd)/Reports":/katalon/report \
		              -e PASSWORD=7024f183-ba93-40a5-bd94-426aa13f9446 \
		              -e PROJECT_ID=1196082 \
		              -e TYPE=katalon \
		              -e REPORT_PATH=/katalon/report \
		              -e PUSH_TO_XRAY=true \
		              katalonstudio/report-uploader:0.0.8
		          '''
		        }
     		 }
   		 }
    }
    post {
        always {
            archiveArtifacts artifacts: 'Reports/**/*.*', fingerprint: true
            junit 'Reports/**/JUnit_Report.xml'
        }
    }
    
}
