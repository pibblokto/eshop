pipeline {
    agent any
    stages {
        stage(“Build_Application”) {
            parallel {
                stage(“build_services”) {
                    agent any
                    steps {
                        sh '''
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            cd src
                            sudo /usr/local/bin/docker-compose build identity-api basket-api catalog-api ordering-api payment-api webhooks-api
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/identity.api:latest
                            docker push gcr.io/eshop-infrastructure/basket.api:latest
                            docker push gcr.io/eshop-infrastructure/catalog.api:latest
                            docker push gcr.io/eshop-infrastructure/ordering.api:latest
                            docker push gcr.io/eshop-infrastructure/payment.api:latest
                            docker push gcr.io/eshop-infrastructure/webhooks.api:latest
                        '''
                        }
                }
                stage(“build_web”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webhooks-client webmvc webspa webstatus
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webhooks.client:latest
                        docker push gcr.io/eshop-infrastructure/webmvc:latest
                        docker push gcr.io/eshop-infrastructure/webspa:latest
                        docker push gcr.io/eshop-infrastructure/webstatus:latest
                    '''
                    }

                }
                stage(“build_dockerhub”) {
                    agent any
                    steps {
                        sh ''' 
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            cd src
                            sudo /usr/local/bin/docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/seq:latest
                            docker push gcr.io/eshop-infrastructure/mongo:latest
                            docker push gcr.io/eshop-infrastructure/redis:latest
                            docker push gcr.io/eshop-infrastructure/rabbitmq:latest
                            docker push gcr.io/eshop-infrastructure/mobileshoppingapigw:latest
                            docker push gcr.io/eshop-infrastructure/webshoppingapigw:latest
                        '''
                        }
                }
                stage(“build_apigw”) {
                    agent any
                    steps {
                           sh '''
                           sudo chmod +x /usr/local/bin/docker-compose
                           sudo chmod 777 /var/run/docker.sock
                           cd src
                           sudo /usr/local/bin/docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                           echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                           docker push gcr.io/eshop-infrastructure/ordering.backgroundtasks:latest
                           docker push gcr.io/eshop-infrastructure/mobileshoppingagg:latest
                           docker push gcr.io/eshop-infrastructure/webshoppingagg:latest
                           docker push gcr.io/eshop-infrastructure/ordering.signalrhub:latest
                       '''
                       }
                }
            }
        }
    }
}





