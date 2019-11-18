pipeline {
    agent { label "production" }

    stages {
        stage("Bundle") {
            steps {
                withAWS(profile:"JUMBO-ACCOUNT") {
                    script {
                        def branch = env.GIT_BRANCH.replaceAll("(.*)/", "")
                        sh "docker build -t zdp-airflow:${branch} ."
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    withAWS(profile:"JUMBO-ACCOUNT") {
                        script {
                            def branch = env.GIT_BRANCH.replaceAll("(.*)/", "")
                            sh "\$(aws ecr get-login --no-include-email --region ap-southeast-1)"
                            sh "docker tag zzdp-airflow:${branch} 125719378300.dkr.ecr.ap-southeast-1.amazonaws.com/zdp-airflow:${branch}"
                            sh "docker push 125719378300.dkr.ecr.ap-southeast-1.amazonaws.com/zdp-airflow:${branch}"
                        }
                    }
                }
            }
        }
    }
}