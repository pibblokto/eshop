pipeline {
    agent any
     environment {                 
        TAG = ":main.${sh(echo $RANDOM | md5sum | head -c 20;)}"
        }
    stages {
        stage(“Build_Application”) {
            stage("generate_tag") {
                
            }
            parallel {
                stage(“build_services”) {
                    agent any
                    steps {
                        sh '''
                            sudo chmod +x /usr/local/bin/docker-compose
                            sudo chmod 777 /var/run/docker.sock
                            cd src
                            sudo /usr/local/bin/docker-compose build identity-api basket-api catalog-api orderingapi paymentapi webhooks-api
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/identity.api${TAG}
                            docker push gcr.io/eshop-infrastructure/basket.api${TAG}
                            docker push gcr.io/eshop-infrastructure/catalog.api${TAG}
                            docker push gcr.io/eshop-infrastructure/ordering.api${TAG}
                            docker push gcr.io/eshop-infrastructure/payment.api${TAG}
                            docker push gcr.io/eshop-infrastructure/webhooks.api${TAG}
                        '''
                        }
                }
                stage(“build_web1”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webhooks-client
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webhooks.client${TAG}
                    '''
                    }

                }
                stage(“build_web2”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webmvc
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webmvc${TAG}
                    '''
                    }

                }
                stage(“build_web3”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webspa
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webspa${TAG}
                    '''
                    }

                }
                stage(“build_web4”) {
                    agent any
                    steps {
                        sh '''
                        sudo chmod +x /usr/local/bin/docker-compose
                        sudo chmod 777 /var/run/docker.sock
                        cd src
                        sudo /usr/local/bin/docker-compose build webstatus
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webstatus${TAG}
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
                            docker push gcr.io/eshop-infrastructure/seq${TAG}
                            docker push gcr.io/eshop-infrastructure/mongo${TAG}
                            docker push gcr.io/eshop-infrastructure/redis${TAG}
                            docker push gcr.io/eshop-infrastructure/rabbitmq${TAG}
                            docker push gcr.io/eshop-infrastructure/mobileshoppingapigw${TAG}
                            docker push gcr.io/eshop-infrastructure/webshoppingapigw${TAG}
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
                           docker push gcr.io/eshop-infrastructure/ordering.backgroundtasks${TAG}
                           docker push gcr.io/eshop-infrastructure/mobileshoppingagg${TAG}
                           docker push gcr.io/eshop-infrastructure/webshoppingagg${TAG}
                           docker push gcr.io/eshop-infrastructure/ordering.signalrhub${TAG}
                       '''
                       }
                }
            }
        }
    }
}