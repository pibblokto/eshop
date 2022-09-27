pipeline {
    agent any
     environment {                 
        HASH = """${sh(
                returnStdout: true,
                script: 'echo $RANDOM | md5sum | head -c 20;'
            )}"""
     }
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
                            echo "HASH=${HASH}" > .env
                            cat .env
                            sudo /usr/local/bin/docker-compose build identityapi basketapi catalogapi orderingapi paymentapi webhooksapi
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/identityapi:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/basketapi:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/catalogapi:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/orderingapi:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/paymentapi:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/webhooksapi:main.${HASH}
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
                        echo "HASH=${HASH}" > .env
                        cat .env
                        sudo /usr/local/bin/docker-compose build webhooksclient
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webhooks.client:main.${HASH}
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
                        echo "HASH=${HASH}" > .env
                        cat .env
                        sudo /usr/local/bin/docker-compose build webmvc
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webmvc:main.${HASH}
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
                        echo "HASH=${HASH}" > .env
                        cat .env
                        sudo /usr/local/bin/docker-compose build webspa
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webspa:main.${HASH}
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
                        echo "HASH=${HASH}" > .env
                        cat .env
                        sudo /usr/local/bin/docker-compose build webstatus
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webstatus:main.${HASH}
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
                            echo "HASH=${HASH}" > .env
                            cat .env
                            sudo /usr/local/bin/docker-compose build seq nosqldata basketdata rabbitmq mobileshoppingapigw webshoppingapigw
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/seq:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/mongo:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/redis:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/rabbitmq:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/mobileshoppingapigw:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/webshoppingapigw:main.${HASH}
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
                           echo "HASH=${HASH}" > .env
                           cat .env
                           sudo /usr/local/bin/docker-compose build orderingbackgroundtasks mobileshoppingagg webshoppingagg orderingsignalrhub
                           echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                           docker push gcr.io/eshop-infrastructure/orderingbackgroundtasks:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/mobileshoppingagg:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/webshoppingagg:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/orderingsignalrhub:main.${HASH}
                       '''
                       }
                }
            }
        }
    }
}