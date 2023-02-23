pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
                git branch:'main', url:'https://github.com/miteshkothari259/MLOPS'
            }
        }
        
        stage('docker permission')
        { 
            steps{
                sh "sudo chmod 666 /var/run/docker.sock"
            }
         }
        stage('build docker image')
        {
            steps{
                sh 'docker build -t my-docker .'
            }
        }
        stage("check the image is bulit or not")
        {
            steps{
                sh '''
                if (sudo docker ps -l | grep my-docker)
                then
	                echo "my docer built"
                else 
                	echo "my docker not built"
                fi'''
            }
        }
        stage("docker run")
        {
            steps{
                sh 'sudo docker run -p 5000:5000 my-docker python3 train-nn.py > nn_result.txt'
            }
        }
        stage('Ml run'){
            steps{
                sh '''
                res=$(sudo cat nn_result.txt)
                test=80

                if [ $test -gt $res ]
                then
	                echo "accuracy not superior to 80%"
                    sudo cat nn_result.txt
                else
	                sudo docker run -p 5000:5000 my-docker python3 train-auto-nn.py > auto_result.txt
	                sudo cat auto_result.txt

                fi   
                '''
            }
        }
}
}
