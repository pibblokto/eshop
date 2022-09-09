pipeline {
    agent any
    stages {
        stage(“Build_Application”) {
            parallel {
                stage(“build_services”) {
                    agent any
                    steps {
                        sh '''
                            sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo /usr/local/bin/docker-compose build identity-api basket-api catalog-api ordering-api payment-api webhooks-api
                            sudo docker push gcr.io/eshop-infrastructure/identity.api:latest
                            sudo docker push gcr.io/eshop-infrastructure/basket.api:latest
                            sudo docker push gcr.io/eshop-infrastructure/catalog.api:latest
                            sudo docker push gcr.io/eshop-infrastructure/ordering.api:latest
                            sudo docker push gcr.io/eshop-infrastructure/payment.api:latest
                            sudo docker push gcr.io/eshop-infrastructure/webhooks.api:latest
                        '''
                        }
                }
                stage(“build_web”) {
                    agent any
                    steps {
                        sh '''
                        sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        cd src
                        sudo /usr/local/bin/docker-compose build webhooks-client webmvc webspa webstatus
                        sudo docker push gcr.io/eshop-infrastructure/webhooks.client:latest
                        sudo docker push gcr.io/eshop-infrastructure/webmvc:latest
                        sudo docker push gcr.io/eshop-infrastructure/webspa:latest
                        sudo docker push gcr.io/eshop-infrastructure/webstatus:latest
                    '''
                    }

                }
                stage(“build_dockerhub”) {
                    agent any
                    steps {
                        sh '''
                            sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo /usr/local/bin/docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            sudo docker push gcr.io/eshop-infrastructure/seq:latest
                            sudo docker push gcr.io/eshop-infrastructure/mongo:latest
                            sudo docker push gcr.io/eshop-infrastructure/redis:latest
                            sudo docker push gcr.io/eshop-infrastructure/rabbitmq:latest
                            sudo docker push gcr.io/eshop-infrastructure/mobileshoppingapigw:latest
                            sudo docker push gcr.io/eshop-infrastructure/webshoppingapigw:latest
                        '''
                        }
                }
                stage(“build_apigw”) {
                    agent any
                    steps {
                           sh '''
                           sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                           sudo chmod +x /usr/local/bin/docker-compose
                           sudo chmod 777 /var/run/docker.sock
                           sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                           cd src
                           sudo /usr/local/bin/docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                           sudo docker push gcr.io/eshop-infrastructure/ordering.backgroundtasks:latest
                           sudo docker push gcr.io/eshop-infrastructure/mobileshoppingagg:latest
                           sudo docker push gcr.io/eshop-infrastructure/webshoppingagg:latest
                           sudo docker push gcr.io/eshop-infrastructure/ordering.signalrhub:latest
                       '''
                       }
                }
            }
        }
    }
}