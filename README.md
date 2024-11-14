# Access INRIX Data for AWS x INRIX AI Hack 2024

This repo demonstrates how to set up a web server using Flask to access live traffic camera data from INRIX and fetch data from an S3 Bucket for training AI/ML Models.

## 1) Breaking Down Postman
- Postman is an industry tool for making API calls and interpreting their outputs. Use Postman on the [web](https://postman.com) or the VSCode extension
- Import the Postman collection `INRIX Access.postman_collection.json` to see the 3 requests you will need to get the live data

### 1a) Security Token API
- Required parameters: None
- Simply call the API and it returns a token to be used for the other 2 APIs.
- The token needs to be refreshed every hour, which is your job!

### 1b) Cameras In A Box API
- Required parameters: token, corner1, corner2
- Given a geobox in latitude and longitude, returns the ID, latitude, and longitude of every camera in that box
- Returned as XML, which Flask API parses automatically
  
### 1c) Camera Image API
- Required parameters: token, camera id
- Given a camera id, returns the real-time location image of that camera in 640x480
- Returns an image, very different than most return types so take your time with it

## 2) Setting up Python environment and running the Flask server
First, we need to create a virtual environment for Python, which essentially installs your packages to 1 project instead of globally

`python3 -m venv venv`

Then, you need to actually use the environment

`source ./venv/bin/activate`

Your terminal should have (venv) at the beginning of it now! Also set your VSCode interpreter to the venv path in the bottom right

I have made a requirements.txt to install all of the packages you will need, so run the following to install them

`pip install -r requirements.txt`

### 2a) Breaking down what is happening
Feel free to poke around Token, CamerasBox, and CameraImage .pys to see what's going on under the hood, but we really only care aboust `server.py`

It comes with 3 endpoints: /token, /cameras, and /camera-image. These can be accessed by running `python3 server.py` and typing localhost:3000/{endpoint} in your browser. More specifics are in the comments of the file

These will return JSON that use can in your frontend or other calls, with the final call being for the camera image that returns an image. You can use this in your AI model to run a predicition or for analysis!

# 3) Getting sample data to train an AI/ML Model
- This is in an S3 bucket hosted on AWS, so this can easily be used as a dataset for SageMaker or Bedrock all through AWS!
- If you need the files however, s3access and s3download .pys are useful for you
- The credentials should've been shared with you, you will need the INRIX access key, secret, and bucket name
- Simply run the files that you need
  - s3access to see the files
  - s3downlaod to download a specific file
- This dataset contains images of traffic for 24 hours, every 5 minutes, on all cameras in Seattle. This is great for training an AI model on detection of many different things regarding traffic!