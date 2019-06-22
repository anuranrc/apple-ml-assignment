# Apple-ML-Deployment Assignment

## Interacting with the API

Python based `Flask` microframework has been used in this assignment to create the HTTP server. 
There are two scripts in the module: 
1. ml_server.py
2. ml_request.py

### Starting the Server from the terminal on a local machine ###

To start the server from the local machine
1. Clone this repo:
```
git clone https://github.com/anuranrc/apple-ml-assignment.git
```
2. `cd` into the folder:
```
cd apple-ml-assignment
```
3. Open up a terminal
4. Start the server with the following command:
```
python src/server/ml_server.py
# This will start the server on http://localhost:5000
```
Please note that you may need to have the dependencies installed on your local machine for this to be successful. The dependencies include:
* Keras
* Flask
* Tensorflow
* numpy

### Starting the server from the AWS EC2 Instance Docker (Deep Learning Docker Container) ###
1. Create an AWS account
2. Launch an EC2 instance: for instance, choose 'AWS Deep Learning Base AMI'
3. Select t2.micro for free tier general purpose computation
4. Create Key set for access
5. Connect to the instance from your local machine: (Connection mechanism varies a little depending on your OS. Open up a terminal on Mac or Linux. For Windows open up Git Bash)
```
ssh -i /path/to/pem/file.pem ubuntu@public.dns.url.of.instance
```
Example:
```
ssh -i ml_apple.pem ubuntu@ec2-107-23-237-121.compute-1.amazonaws.com
```
6. Optional:
Now after SSH login succeeds, type:
```
aws configure
```
Put the IAM access key and the secret key to login to Amazon ECR with the following command:
```
$(aws ecr get-login --region us-east-1 --no-include-email --registry-ids 763104351884)
```
If succeeded, it should show `Login Succeeded`

7. Running the docker:
```
docker run -it 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-cpu-py36-ubuntu16.04
```
This will run a Deep Learning Docker Image

8. Clone this repo:
```
git clone https://github.com/anuranrc/apple-ml-assignment.git
```
9. `cd` into the folder:
```
cd apple-ml-assignment
```
10. Start the server with the following command:
```
python src/server/ml_server.py
# This will start the server on http://localhost:5000
```
Note: If you experience this error: `ModuleNotFoundError: No module named 'flask'` then simply install `flask` in your docker environment by:
```
pip install flask
```

### `POST` API call to `/predict` route ###

## Using cURL command ##
1. Open another terminal within the same directory and type the following cURL command:
```
curl -X POST -F image=@'path/to/image.jpg' 'http://localhost:5000/predict'
```
Example: 
```
curl -X POST -F image=@'abc.jpg' 'http://localhost:5000/predict'

or
	
curl -X POST -F image=@'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/image inputs/abc.jpg' 'http://localhost:5000/predict'
```
Description: This is basically a `POST` request which accepts a single image file and returns the predicted results by running the image against the model. The returned value is in a JSON format.
```
# Response:
{
    "predictions": [
        {
            "label": "golden_retriever",
            "probability": 0.7136868834495544
        },
        {
            "label": "Chesapeake_Bay_retriever",
            "probability": 0.08787406235933304
        },
        {
            "label": "vizsla",
            "probability": 0.060452643781900406
        },
        {
            "label": "Brittany_spaniel",
            "probability": 0.03629603236913681
        },
        {
            "label": "flat-coated_retriever",
            "probability": 0.03320077806711197
        }
    ],
    "success": true
}

# The image used here is of a dog that looks like a golden retriever. (See file imgs/dog.jpg)
```

## Running the request file ##
1. In the module there is a sript named `ml_request.py` within `src/helper/request/` folder.
2. Open up a terminal in the project folder and type:
```
python src/helper/request/ml_request.py ./path/to/image.jpg
```
Example:
```
python ml_request.py 'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/input_img/abc.jpg'
# here 'C:/Users/Anuran/Desktop/ml deploy project/git repo folder/apple-ml-assignment/input_img/abc.jpg' is the image path in my local machine.
```
Description: Here a single image is sent as an argument. This is basicallly a `POST` predict request to show the results in more formatted manner.

### `GET` API call to `/getstatus` route ###
1. While the server is running, open up a terminal in the project folder and type:
```
curl http://localhost:5000/getstatus
```
Description: This returns back a JSON with the properties `Current Time` and `Version`:
```
# Response:
{
    "Current Time": "22:09:34",
    "Version": "1.0.1"
}
```

## Summary:
1. Open a terminal in the project folder in your docker container running in the EC2 instance or in your local machine

2. Start the server using:
```
python src/server/ml_server.py
```

3. open another terminal in the same folder and type:
```
python src/helper/request/ml_request.py ./path/to/image.jpg
```
(Give the image path in the imagepath placeholder)
This will show you the results in the terminal

4. Also you can type in the terminal:
```
curl -X POST -F image=@'./path/to/image.jpg' 'http://localhost:5000/predict'
```
(Give the image path in the imagepath placeholder)
(Give the local losturl in the localhost_url placeholder)


