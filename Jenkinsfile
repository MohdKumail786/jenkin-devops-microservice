//scripted
//declartive

pipeline {
	
	    agent any

		environment { 
			dockerHome = tool 'myDocker'
			mavenHome = tool 'myMaven'
			PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
		}

		stages 
		{
           
		   stage ('Checkout'){
			   steps{
                   sh 'mvn --version'
				   sh 'docker version'
                   echo "Build"
				   echo "PATH $PATH"
				   echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				   echo "BUILD_ID - $env.BUILD_ID"
				   echo "JOB_NAME - $env.JOB_NAME"
				   echo "BUILD_TAG - $env.BUILD_TAG"
				   echo "BUILD_URL -$env.BUILD_URL"
			   }
		   }
		   stage ('Compile') {
               steps {

				   sh "mvn clean compile"
			   }
		   }


			stage ('Test'){
			   steps{
                   
				     sh "mvn test"
			   }
			}
			stage ('Integration Test'){
			   steps{

                    sh "mvn failsafe:integration-test failsafe:verify"
			   }
		    }

			stage ('Package'){
			   steps{

                    sh "mvn package -DskipTests"
			   }
		    }

			stage ('Build Docker Image'){
				
				steps {
                       //   docker build -t mkashraf071/currency-exchange-devops:$env.BUILD_TAG
					   script {
						  dockerImage = docker.build("mkashraf071/currency-exchange-devops:${env.BUILD_TAG}")
					   }
				}
			}

			stage ('Push Docker Image'){
				
				steps {
					script {
						docker.withRegistry('', 'dockerHub') {
						dockerImage.push();
						dockerImage.push('latest')
						}
					}
				}
			}
		} 
		
		post {
			always {
				echo 'I am awesome I run always'
			}
			success{
				echo 'I run when you are successfull.'
			}
			failure{
				echo 'I run when you fail dude.'
			}
		}
	//unstable
	//changed
}
