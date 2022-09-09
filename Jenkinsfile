pipeline {
    agent any
    stages {
        stage(“Build_Application”) {
            parallel {
                stage(“build_services”) {
                    steps {
                        sh '''
                            echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            docker-compose build identity-api basket-api catalog-api ordering-api payment-api webhooks-api
                            docker push gcr.io/pelagic-bastion-359618/identity.api:latest
                            docker push gcr.io/pelagic-bastion-359618/basket.api:latest
                            docker push gcr.io/pelagic-bastion-359618/catalog.api:latest
                            docker push gcr.io/pelagic-bastion-359618/ordering.api:latest
                            docker push gcr.io/pelagic-bastion-359618/payment.api:latest
                            docker push gcr.io/pelagic-bastion-359618/webhooks.api:latest
                        '''
                        }
                }
                stage(“build_web”) {
                      steps {
                        sh '''
                            echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            docker-compose build webhooks-client webmvc webspa webstatus
                            docker push gcr.io/pelagic-bastion-359618/webhooks.client:latest
                            docker push gcr.io/pelagic-bastion-359618/webmvc:latest
                            docker push gcr.io/pelagic-bastion-359618/webspa:latest
                            docker push gcr.io/pelagic-bastion-359618/webstatus:latest
                        '''
                        }

                }
                stage(“build_dockerhub”) {
                     steps {
                        sh '''
                            echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            docker push gcr.io/pelagic-bastion-359618/seq:latest
                            docker push gcr.io/pelagic-bastion-359618/mongo:latest
                            docker push gcr.io/pelagic-bastion-359618/redis:latest
                            docker push gcr.io/pelagic-bastion-359618/rabbitmq:latest
                            docker push gcr.io/pelagic-bastion-359618/mobileshoppingapigw:latest
                            docker push gcr.io/pelagic-bastion-359618/webshoppingapigw:latest
                        '''
                        }
                }
                stage(“build_apigw”) {
                     steps {
                        sh '''
                            echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            cd src
                            docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                            docker push gcr.io/pelagic-bastion-359618/ordering.backgroundtasks:latest
                            docker push gcr.io/pelagic-bastion-359618/mobileshoppingagg:latest
                            docker push gcr.io/pelagic-bastion-359618/webshoppingagg:latest
                            docker push gcr.io/pelagic-bastion-359618/ordering.signalrhub:latest
                        '''
                        }
                }
            }
        }
    }
}