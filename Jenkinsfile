
def setupEnv() {
    ["PATH+MAVEN=${tool 'maven3'}/bin",
     "PATH+JAVA_HOME=${tool 'jdk1.8'}/bin",
	 "LT_USERNAME=sujaymasa",
    "LT_ACCESS_KEY=m3EHeBp0sxUhVTwphDoJpQHkm8AQsgdWZXtWcjqmg9sj60GuKs",
    "LT_TUNNEL=false"
     ]
}


def notifySlack(String buildStatus = 'STARTED') {
    
	
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n More info at: ${env.BUILD_URL}"

    slackSend(color: color, channel: '#qa_test', message: msg)
}





node {

	

    
	 try{
        notifySlack()
		
        timestamps {
                    
					
				stage('Compile & Testing') {
				
                withEnv(setupEnv()) {
				
                    checkout scm
                                       

                    try {
					
                        bat 'mvn test -DtestPlanId=${TESTPLAN_KEY}  -Dbrowser=${BROWSER}  -DbrowserVersion=${BROWSER_VERSION} -DosVersion=${OSVERSION}'
      
                    }
                    catch (Exception e) {
                        currentBuild.result = 'FAILURE'
						
						throw e
                    }
                }
            }
            
            
            
        }
    } finally {
        notifySlack(currentBuild.result)
       
    }


 
 

