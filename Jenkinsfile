pipeline {
	environment {
		JOB_NAME = "${env.JOB_NAME}"
	        BUILD_NUMBER = "${env.BUILD_NUMBER}"
	    	}
	agent {
        	label 'puppetagent'
	       	}
	stages {
	    stage('Job123PREPARE') {
	      	steps {
	            	sh 'java -version'
	            	echo 'Puppet Agent Installation & Configuration'
			sh 'sudo apt-get update'
			sh 'sudo apt-get install puppet -y'
			sh 'sudo sh -c "echo [agent] >> /etc/puppet/puppet.conf"'
		 	sh 'sudo sh -c "echo server=ip-172-31-10-251.ap-south-1.compute.internal >> /etc/puppet/puppet.conf"'              
			sh 'sudo puppet agent --enable'
			echo 'Puppet will install Docker & Git ...'
			sh 'sudo puppet agent -t|| true'
			sh 'git --version'
			sh 'docker --version'
			sh 'mkdir /home/ubuntu/jenkin-agent|| true'
		       }
	        }
	    stage('Job4BUILD')   {
	       	steps {
	               	echo 'Docker image build started..'
			sh 'sudo mkdir -p /home/ubuntu/jenkin-agent/rel/${JOB_NAME}${BUILD_NUMBER}'
			sh 'sudo mv /home/ubuntu/jenkin-agent/workspace/devops/website/* /home/ubuntu/jenkin-agent/rel/${JOB_NAME}${BUILD_NUMBER}'
	   		sh 'sudo docker images'
			sh 'sudo docker ps -a'
			sh 'sudo docker pull php:apache'
			sh 'sudo docker stop ${JOB_NAME}${BUILD_NUMBER}||true'
			sh 'sudo docker rm ${JOB_NAME}${BUILD_NUMBER}||true'
			sh 'sudo docker run -d --name=${JOB_NAME}${BUILD_NUMBER} -p 8081:80 -v /home/ubuntu/jenkin-agent/rel/${JOB_NAME}${BUILD_NUMBER}:/var/www/html php:apache'
			sh 'sudo docker images'
			sh 'sudo docker ps -a'
			echo 'Docker image build started..'
	            }
	        }
	    stage('Job4TEST') {
	        steps {
	                echo 'Testing..'
	            }
	        }
	 }
	    post {
	        always {
	            sh 'exit 0'
	               }
                 }
}
