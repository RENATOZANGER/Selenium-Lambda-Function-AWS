# README

## Selenium Lambda Function

This repository contains a Dockerfile and a Python script to create a Lambda function for web scraping using Selenium.

### Dockerfile

The Dockerfile is used to build the Lambda function's environment. It consists of two stages:

1. **build**: This stage installs necessary dependencies and downloads the Chrome browser and Chromedriver.
2. **final**: This stage installs additional system dependencies required for running Chrome in headless mode, installs Selenium, and copies the Chrome browser and Chromedriver binaries from the build stage. It also copies the Python script (`main.py`) into the container.

### Python Script (main.py)

The `main.py` file contains the Lambda function code written in Python. This function uses Selenium to perform web scraping on the provided URL (`https://example.com/`). It initializes a headless Chrome browser using the Chrome browser and Chromedriver binaries, navigates to the specified URL, extracts text content from the HTML, and returns it as the output of the Lambda function.

### Usage

To use this Lambda function, follow these steps:

1. Create a new private repository in Amazon ECR:
- Ex: account_id.dkr.ecr.us-east-1.amazonaws.com/lambda_scraping
2. Push the image to Amazon ECR:
- Retrieve an authentication token and authenticate your Docker client to your registry
- aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin account_id.dkr.ecr.us-east-1.amazonaws.com
3. Build your Docker image
- docker build -t lambda_scraping .
4. Tag your image so you can push
- docker tag lambda_scraping:latest account_id.dkr.ecr.us-east-1.amazonaws.com/lambda_scraping:latest
5. Push this image to your newly created AWS repository
- docker push account_id.dkr.ecr.us-east-1.amazonaws.com/lambda_scraping:latest
6. Create a Lambda function in AWS Lambda.
7. Configure the Lambda function to use the container image from the container registry.
9. set the lambda timeout to more than 1 minute
10. click on test

### Local Testing:

1. Build the image
docker build -t lambda_scraping .

2. Run the image
docker run -p 9000:8080 lambda_scraping:latest

3. Test the running image
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'


