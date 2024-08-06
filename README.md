# Simple Web Application with CI/CD on AWS

## Overview

This project demonstrates a basic web application using Python's Flask framework with a Continuous Integration and Continuous Deployment (CI/CD) pipeline using AWS services. The application is containerized using Docker and deployed using AWS Elastic Beanstalk.

## Project Structure

- `app.py`: The Flask web application.
- `requirements.txt`: Python dependencies.
- `Dockerfile`: Docker configuration for containerizing the application.
- `buildspec.yml`: Build specifications for AWS CodeBuild.
- `README.md`: This README file.

## Prerequisites

- **AWS Account**: You need an AWS account to use AWS CodeCommit, CodeBuild, CodePipeline, and Elastic Beanstalk.
- **AWS CLI**: Ensure you have the AWS CLI installed and configured with your AWS credentials.
- **Docker**: Install Docker if you plan to use the Dockerfile for containerization.

## Setup Instructions

### 1. Create the Web Application

1. **Create a directory for your project:**
    ```sh
    mkdir my-flask-app
    cd my-flask-app
    ```

2. **Create the necessary files:**
    - `app.py`
    - `requirements.txt`
    - `Dockerfile`

3. **Add the code to the files:**
    - `app.py`:
        ```python
        from flask import Flask

        app = Flask(__name__)

        @app.route('/')
        def home():
            return "Hello, AWS CI/CD!"

        if __name__ == '__main__':
            app.run(debug=True, host='0.0.0.0')
        ```
    - `requirements.txt`:
        ```
        Flask==2.0.3
        ```
    - `Dockerfile`:
        ```dockerfile
        FROM python:3.9-slim
        WORKDIR /app
        COPY requirements.txt .
        RUN pip install --no-cache-dir -r requirements.txt
        COPY app.py .
        CMD ["python", "app.py"]
        EXPOSE 5000
        ```

### 2. Set Up AWS CodeCommit

1. **Create a CodeCommit Repository:**
    - Go to the [AWS CodeCommit console](https://console.aws.amazon.com/codesuite/codecommit/home).
    - Create a new repository and name it (e.g., `my-flask-app`).

2. **Push Code to CodeCommit:**
    - Follow the instructions provided by CodeCommit to clone the repository and push your code.

### 3. Configure AWS CodeBuild

1. **Create a Build Project:**
    - Go to the [AWS CodeBuild console](https://console.aws.amazon.com/codesuite/codebuild/home).
    - Create a new build project.
    - Configure the source provider as AWS CodeCommit and select your repository.
    - Set up the build environment with the Python runtime.
    - Create a `buildspec.yml` in your project root with the following content:
        ```yaml
        version: 0.2

        phases:
          install:
            runtime-versions:
              python: 3.9
            commands:
              - echo Installing source NPM packages...
              - pip install -r requirements.txt
          build:
            commands:
              - echo Building the Docker image...
              - docker build -t my-flask-app .
          post_build:
            commands:
              - echo Build completed on `date`
        artifacts:
          files:
            - '**/*'
        ```

### 4. Set Up AWS CodePipeline

1. **Create a Pipeline:**
    - Go to the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/home).
    - Create a new pipeline.
    - Configure the source provider as AWS CodeCommit.
    - Add a build stage and select your CodeBuild project.
    - Add a deploy stage. For this example, choose AWS Elastic Beanstalk.

### 5. Deploy the Application with Elastic Beanstalk

1. **Create an Elastic Beanstalk Application:**
    - Go to the [AWS Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk/home).
    - Create a new application and environment.
    - Choose the Docker platform if you are using Docker or the Python platform if not using Docker.

2. **Deploy Your Application:**
    - Elastic Beanstalk will handle deployment and scaling. Monitor the deployment process in the Elastic Beanstalk console.

## Additional Notes

- **Customizing the Dockerfile**: If you need additional configurations or dependencies, modify the Dockerfile accordingly.
- **Elastic Beanstalk Environment**: Ensure that the environment configuration matches your application’s requirements.

## Troubleshooting

- **CodeBuild Issues**: Check the build logs in the AWS CodeBuild console for detailed error messages.
- **Elastic Beanstalk Deployment**: Review the Elastic Beanstalk logs if the deployment fails or the application is not accessible.

## Conclusion

You now have a basic setup for a web application with CI/CD on AWS. This guide covers the essential steps to get your application up and running with automated deployments. Feel free to expand upon this setup as needed for your specific use case.

For more details, refer to the [AWS Documentation](https://docs.aws.amazon.com/) for CodeCommit, CodeBuild, CodePipeline, and Elastic Beanstalk.
