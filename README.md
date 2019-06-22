# Apple-Ml-Deployment Assignment

## Interacting with the API

To create the HTTP server python based Flask microframework has been used. 
There are basically two scripts in the module: 
1. ml_server.py
2. ml_request.py

##### HTTTP Server #####

### Starting the Server from the terminal in local ###

To start the server from the local machine
 1. Go to the repo folder
 2. open a terminal
 3. Type-> python ml_server.py    -->This will start the server in a local port

### Starting the server from the AWS EC2 Instance Docker (Deep Learning Docker Container) ###
	1. Create an AWS account
	2. Launch an EC2 instance: for instance: choose 'AWS Deep Learning Base AMI'
	3. Select t2.micro for free tier general purpose computation
	4. Create Key set for access
	5. Connect to the instance from your local machine: (Connection mechanism varies a little depending on the local machine)
	Normally open a git bash terminal in the Desktop and type
	ssh -i pem fodler location ubuntu@public_dns_url

	# Put the path of your private key file in the 'pem _file_location' and the public DNS url of your instance in the public_dns_url

	example: ssh -i pem_file_location ubuntu@ec2-107-23-237-121.compute-1.amazonaws.com

	Optional:
	Now after SSH log in succeed Type-> aws configure
	put the IAM access key and the secret key to login to Amazon ECR

	Now type--> $(aws ecr get-login --region us-east-1 --no-include-email --registry-ids 763104351884)
	and it will show login suceeded


	6. Running the docker:
	Then type--> 
	docker run -it 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-cpu-py36-ubuntu16.04
	This will run a Deep Learning Docker Image

	7. clone the repo provided in the gihub for this assignment.

	8. Now start the Server just like how you do it from the local machine.
	Inside the repo directory
	Type--> python ml_server.py


### Sending the POST requst to show the predicted results ###

	### By sending the request from the terminal ###
		 1. Then open another terminal in the same directory
		 2. Type-->  curl -X POST -F image=@'imagepath' 'localhost_url/predict'
		 example: 
		 curl -X POST -F image=@abc.jpg 'http://localhost:5000/predict'

		 or 

		 curl -X POST -F image=@'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/image inputs/abc.jpg' 'http://localhost:5000/predict'

		 Description: This is basically a POST request which accepts a single image and returns the predicted results by running the image against the model in a JSON format

	### By sending the request through running a script ###
		1. In the module there is a sript named 'ml_request.py' 
		2. Open a terminal in the project folder
		3. Type->  python ml_request.py imagepath
		4. 	Give the image path in the imagepath placeholder. 

		example: python ml_request.py 'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/input_img/abc.jpg'
		here 'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/input_img/abc.jpg' is the image path in my local machine.

		If it is in the same folder just type the name.
		example: python ml_request.py abc.jpg

		Description: Here a single image is send as an argument. This is basicallly a POST predict request to show the results in more formatted manner.



## Summary: 
1. Open a terminal in the project folder in your docker container running in the EC2 instance or in your local machine

2. Start the server using: python ml_server.py

3. open another terminal in the same folder and type:
ml_request.py imagepath
(Give the image path in the imagepath placeholder)
This will show you the results in the terminal

4. Also you can type in the terminal:
curl -X POST -F image=@'imagepath' 'localhost_url/predict'
(Give the image path in the imagepath placeholder)
(Give the local losturl in the localhost_url placeholder)


