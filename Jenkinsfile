pipeline {
   agent any
   
   environment {
		KLOCWORK_URL = "http://localhost:8080"
		KLOCWORK_PROJECT = "zlib-pipeline"
		KLOCWORK_LICENSE_HOST = "localhost"
		KLOCWORK_LICENSE_PORT = "27000"
		KLOCWORK_LTOKEN = ""		
    }
   
   stages {
       /* cool
	   stage('Get src from git') {
			 steps {
				git credentialsId: '19532756-711b-4217-b216-06ef0f03cd26', url: 'https://github.com/butylin/zlib-local.git'
				echo "my commit"
				echo "${env.GIT_PREVIOUS_COMMIT}"
				echo "${env.GIT_CHECKOUT_DIR}"
				echo "job ${env.JOB_URL}"
			 }
	    }
		*/
	    
        stage('Booo') {
            steps {
                
		   env.MYVAR = checkout(scm)
		   echo "scmVars: ${env.MYVAR}"
		    
		/*    
		final scmVars = checkout(scm)
		echo "scmVars: ${scmVars}"
		echo "scmVars.GIT_COMMIT: ${scmVars.GIT_COMMIT}"
		echo "scmVars.GIT_BRANCH: ${scmVars.GIT_BRANCH}"
                */
                                
				echo "env branch name ${env.BRANCH_NAME}"
				echo "env prev commit ${env.GIT_PREVIOUS_COMMIT}"
				echo "env commit ${env.GIT_COMMIT}"  
            }
        }
	    
                
	    
	    stage('Klocwork Build') {
			 steps {
				 
					klocworkBuildSpecGeneration([additionalOpts: '', buildCommand: 'c:\\dev\\zlib-git.bat', ignoreErrors: true, output: 'kwinject.out', tool: 'kwinject'])
				
			 }
		}
		  
		stage('Klocwork Ci Analysis') {
			steps {
				 
					 klocworkIncremental([additionalOpts: '', buildSpec: 'kwinject.out', cleanupProject: true, differentialAnalysisConfig: [diffFileList: 'diff_file_list.txt', diffType: 'git', gitPreviousCommit: '${GIT_PREVIOUS_COMMIT}'], incrementalAnalysis: true, projectDir: '', reportFile: ''])
				 
			 }
		  }

		  
		  stage('Quality gateway') {
			 steps {
				 
					 klocworkFailureCondition([enableCiFailureCondition: true, failureConditionCiConfigs: [[enableHTMLReporting: true, enabledStatuses: [analyze: true, defer: true, filter: true, fix: true, fixInLaterRelease: true, fixInNextRelease: true, ignore: true, notAProblem: true], failUnstable: true, name: 'one or more', reportFile: '', threshold: '1']]])
				 
			 }
		  }
		  
		  
   }
   
			
}
