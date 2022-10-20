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
    }
   
}
