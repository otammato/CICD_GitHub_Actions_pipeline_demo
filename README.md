# GitHub_Actions_pipeline_demo

[ CI/CD Docker Java app ] Create a CI/CD pipeline with GitHub Actions buills the app, then built and push the Docker image

<br><br>

## Automatically build app and push the image to the Docker Hub:

```
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will build a Java project with Gradle and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

# After being built, the Docker image is built and pushed to the Docker hub

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

### This is how to setup GitHub and store the DockerHub access token:

<br><br>
<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 10 56 11" src="https://user-images.githubusercontent.com/104728608/219629685-b787134e-16c5-4930-9ce0-c07f46877209.png">
</p>
<br><br>


<br><br>
<p align="center" >
  <img width="700" alt="Screenshot 2023-02-17 at 10 40 20" src="https://user-images.githubusercontent.com/104728608/219625540-382f454c-d371-4b72-bba3-a80291f3f9af.png">
</p>
<br><br>


<br><br>
## or Manually build app and push the image to the Docker Hub:

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
