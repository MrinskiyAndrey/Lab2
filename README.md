# Технология разработки программного обеспечения
#### Лабораторная работа № 2: создание кластера Kubernetes и деплой приложения
#### Мринский Андрей Николаевич, группа МБД2131
#### Целью лабораторной работы является знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер.
#
## **Манифесты deployment.yaml и service.yaml в тексте страницы**

### deployment.yaml
####
#### 
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: my-deployment
        spec:
          replicas: 2
          selector:
            matchLabels:
              app: my-app
          strategy:
            rollingUpdate:
              maxSurge: 1
              maxUnavailable: 1
            type: RollingUpdate
          template:
            metadata:
              labels:
                app: my-app
            spec:
              containers:
                - image: myapi:latest
                  # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
                  # указыаает на то, что образы нужно брать только из локального registry. В продакшене никогда не использовать
                  imagePullPolicy: Never 
                  name: myapi
                  ports:
                    - containerPort: 8080
              hostAliases:
              - ip: "192.168.49.1" # The IP of localhost from MiniKube
                hostnames:
                - postgres.local

### service.yaml
####
####
        apiVersion: v1
        kind: Service
        metadata:
          name: my-service
        spec:
          type: NodePort
          ports:
            - nodePort: 31317
              port: 8080
              protocol: TCP
              targetPort: 8080
          selector:
            app: my-app
            
  ##
  
 ## Скриншот подов в консоли
  ![Image alt](https://github.com/MrinskiyAndrey/Lab2/blob/458dbbbb2ef4c2369133919944bc336bf55dfff2/Lab2%20-1.png)
  
  #
  ##Скриншот подов в графическом интерфейса
  ![Image alt](https://github.com/MrinskiyAndrey/Lab2/blob/8095b75d23dece5f5b0110932930dc39f6888fa3/Lab2-2.png)
  
  
  
            
