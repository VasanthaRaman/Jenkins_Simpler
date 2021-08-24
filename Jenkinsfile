pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "mavenn"
    }

    stages {
    	stage('Git checkout') {
    		steps {
    			git 'https://github.com/VasanthaRaman/Jenkins_Simpler.git'
    		}
    	}
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                
                 echo "Running Job: ${env.JOB_NAME}\n build: ${env.BUILD_ID}"

                // Run Maven on a Unix agent.
               // sh "mvn -Dmaven.test.failure.ignore=true clean package"
               
               sh 'mvn -f ./pom.xml clean install package'
              
              

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
             post{
                success{
                    archiveArtifacts(artifacts: 'target/*.war', allowEmptyArchive: true)
                }
            }

         //   post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
               // success {
            //        junit '**/target/surefire-reports/TEST-*.xml'
             //       archiveArtifacts 'target/*.jar'
            //    }
          //  }
        }
        
    }
    
    post{
		failure{
                mail to: 'vasantharaman003@gmail.com',
                from: 'vasantharaman003@gmail.com',
                subject: "Project Build: ${env.JOB_NAME} - Failed",
                body: "Job Failed -  \"${env.JOB_NAME} \" build: ${env.BUILD_NUMBER}"
            }
		success{
                    emailext attachmentsPattern: "*target/*.war",
                    body: '''${SCRIPT, template="groovy-template"}''',
                    subject: "Project Build: ${env.JOB_NAME} - SUCCESS",
                    mimeType: 'text/html',
                    to: 'vasantharaman003@gmail.com'
            }
	}
}
