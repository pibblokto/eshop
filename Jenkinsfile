pipeline {
    agent any
     environment {                 
        HASH = """${sh(
                returnStdout: true,
                script: 'echo $RANDOM | md5sum | head -c 20;'
            )}"""
     }
    stages {
        stage(“save_tag”) {
                    agent any
                    steps {
                        sh '''
                            echo "main.${HASH}" > tag.txt
                            sudo aws s3 cp ./tag.txt s3://${BucketName}
                        '''
                        }
                }

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
                            sudo /usr/local/bin/docker-compose build identity-api basket-api catalog-api ordering-api payment-api webhooks-api
                            sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                            docker push gcr.io/eshop-infrastructure/identity-api:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/basket-api:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/catalog-api:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/ordering-api:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/payment-api:main.${HASH}
                            docker push gcr.io/eshop-infrastructure/webhooks-api:main.${HASH}
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
                        sudo /usr/local/bin/docker-compose build webhooks-client
                        sudo echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                        docker push gcr.io/eshop-infrastructure/webhooks-client:main.${HASH}
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
                           sudo /usr/local/bin/docker-compose build ordering-backgroundtasks mobileshoppingagg webshoppingagg ordering-signalrhub
                           echo $GOOGLE_CLOUD_ACCOUNT | docker login -u _json_key --password-stdin https://gcr.io
                           docker push gcr.io/eshop-infrastructure/ordering-backgroundtasks:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/mobileshoppingagg:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/webshoppingagg:main.${HASH}
                           docker push gcr.io/eshop-infrastructure/ordering-signalrhub:main.${HASH}
                       '''
                       }
                }
            }
        }
    }
}