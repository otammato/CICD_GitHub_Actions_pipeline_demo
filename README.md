# GitHub_Actions_pipeline_demo

[ CI/CD Docker Java ] Create a CI/CD pipeline with GitHub Actions buills the app, then built and push the Docker image

<br><br>

## Automatically build the app and push the image to the Docker Hub:
### The YAML file for automation:
```
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will build a Java project with Gradle and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

# After the app being built, the Docker image is built and pushed to the Docker hub

name: Java CI with Gradle

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        arguments: build
        
    - name: Build and Push Docker image
      uses: mr-smithers-excellent/docker-build-push@v4
      with:
        image: montcarotte/cicd_demo_github_actions
        registry: docker.io
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
```
<br><br>

### Configure the GitHub actions and modify the YAML scenario to build and push Docker images (paste the scenario from the above step)
<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 11 46 07" src="https://user-images.githubusercontent.com/104728608/219647257-9e959e8e-ce40-4773-a6c1-17f2c83197cb.png">
</p>
<br><br>



<br><br>
### This is how to set up and store the DockerHub access token on GitHub:

<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 10 56 11" src="https://user-images.githubusercontent.com/104728608/219629685-b787134e-16c5-4930-9ce0-c07f46877209.png">
</p>
<br><br>


<br><br>
<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 10 40 20" src="https://user-images.githubusercontent.com/104728608/219625540-382f454c-d371-4b72-bba3-a80291f3f9af.png">
</p>
<br><br>



### Demonstration of how the pipepline works on any push on "main" or a merge and its result on the DockerHub:
<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 11 11 10" src="https://user-images.githubusercontent.com/104728608/219634999-a8da3b1b-6132-4347-b05e-7ba146ee678a.png">
</p>
<br><br>

<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 11 12 24" src="https://user-images.githubusercontent.com/104728608/219634657-be5e0fa9-d96e-4adf-8a0b-f2134433d5bd.png">
</p>
<br><br>

<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 11 16 03" src="https://user-images.githubusercontent.com/104728608/219636356-2c8f9fa4-a210-43ba-9aa5-cbad333fa713.png">
</p>
<br><br>



<br><br>
## This is how to manually build the app and push the image to Docker Hub:

##### build the project
```
./gradlew build
```
##### build Docker image called java-app. Execute from root
```
docker build -t java-app .
```    
##### push image to repo 
```
docker tag java-app demo-app:java-1.0
```   
