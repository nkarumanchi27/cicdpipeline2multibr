pipeline { 
  
   agent any

   stages {
   
     stage('Install Dependencies') { 
        steps { 
           sh 'echo "run npm install here for stage branch-webhook"' 
        }
     }
     
     stage('Test') { 
        steps { 
           sh 'echo "testing application..."'
        }
      }

     stage("Deploy application") { 
         steps { 
           sh 'echo "deploying application..."'
         }

     }
    stage('run on stage'){
        when {

          branch "stage"

        }
        steps {
           sh 'echo "This step will only run in stage environment branch"'
        }

    }
    stage('Run on master branch'){
	when {
          branch "master"
        }
        steps {
          sh 'echo " THis step will only run in the master branch environment"'
  
   	}

   }

 }

}

