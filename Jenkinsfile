pipeline{
    agent{
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages{
        stage("Build"){
            steps{
                echo "========executing Build stage========"
                sh 'mvn -B -DskipTests clean package'
            }
        }
	stage("Deploy"){
            steps{
                echo "====++++executing Deploy++++===="
                sh './jenkins/scripts/deliver.sh'
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Deploy executed successfully++++===="
                }
                failure{
                    echo "====++++Deploy execution failed++++===="
                }
       
            }
        }
	stage("Test"){
            steps{
                echo "========executing Test stage========"
                sh 'mvn test'
            }
            post{
                always{
                    echo "========executing always stage========"
                    junit 'target/surefire-reports/*xml'
                }
                success{
                    echo "========pass stage========"
                }
                failure{
                    echo "========failed stage========"
                }
            }
        }
	    stage("Sonarqube Analysis"){
	    steps{
		echo "====++++executing A++++===="
		withSonarQubeEnv('SonarQube') {
		sh 'mvn sonar:sonar'
	    }
	    }
	    post{
		always{
		    echo "====++++always++++===="
		}
		success{
		    echo "====++++A executed successfully++++===="
		}
		failure{
		    echo "====++++A execution failed++++===="
		}

	    }
	}
    }
   
}
