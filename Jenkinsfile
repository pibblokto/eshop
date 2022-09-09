pipeline {
    agent any
    stages {
        stage(“Build_Application”) {
            parallel {
                stage(“build_services”) {
                    steps {
                        sh '''
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo docker-compose build identity-api basket-api catalog-api ordering-api payment-api webhooks-api
                            sudo docker push gcr.io/pelagic-bastion-359618/identity.api:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/basket.api:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/catalog.api:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/ordering.api:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/payment.api:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webhooks.api:latest
                        '''
                        }
                }
                stage(“build_web”) {
                      steps {
                        sh '''
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo docker-compose build webhooks-client webmvc webspa webstatus
                            sudo docker push gcr.io/pelagic-bastion-359618/webhooks.client:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webmvc:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webspa:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webstatus:latest
                        '''
                        }

                }
                stage(“build_dockerhub”) {
                     steps {
                        sh '''
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            sudo docker push gcr.io/pelagic-bastion-359618/seq:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/mongo:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/redis:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/rabbitmq:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/mobileshoppingapigw:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webshoppingapigw:latest
                        '''
                        }
                }
                stage(“build_apigw”) {
                     steps {
                        sh '''
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            sudo docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                            sudo docker push gcr.io/pelagic-bastion-359618/ordering.backgroundtasks:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/mobileshoppingagg:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/webshoppingagg:latest
                            sudo docker push gcr.io/pelagic-bastion-359618/ordering.signalrhub:latest
                        '''
                        }
                }
            }
        }
    }
}