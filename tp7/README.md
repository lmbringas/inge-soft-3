# Servidor de Build (de integración continua)

[Repositorio del proyecto spring](https://github.com/lmbringas/spring-boot-test)

## Pipline con docker 


```
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git(url: 'https://github.com/lmbringas/spring-boot-test.git', branch: 'main', poll: true)
                sh "docker build -t lautibringas/spring-boot:${env.BUILD_ID} ."
            }
        }
        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "docker login -u $USER -p $PASS"
                    sh "docker push lautibringas/spring-boot:${env.BUILD_ID}"
                }
            }
        }
    }
}

```

# Resultado


 ![Output](assets/dockerhub.png)