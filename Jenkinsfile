pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-pat', url: 'https://github.com/KarolStoinski/abcd-student.git', branch: 'main'
                }
            }
        }
        stage('SCA scan') {
            steps {
                sh 'mkdir results'
                sh 'osv-scanner scan --lockfile package-lock.json --format json --output results/sca-osv-scanner.json || true'
            }
            post {
                always {
                    defectDojoPublisher(artifact: 'results/sca-osv-scanner.json', 
                            productName: 'Juice Shop', 
                            scanType: 'OSV Scan', 
                            engagementName: 'karol.stoinski@gmail.com')
                }
            }
        }
    //     stage('[ZAP] Baseline passive-scan') {
    //         steps {
    //             sh 'mkdir -p results/'
    //             sh '''
    //                 docker run --name juice-shop -d --rm \
    //                     -p 3000:3000 \
    //                     bkimminich/juice-shop
    //                 sleep 5
    //             '''
    //             sh '''
    //                 docker run --name zap \
    //                     --add-host=host.docker.internal:host-gateway \
    //                     -v "/Users/karolstoinski/BezpiecznyKod/abcd-student":/zap/wrk/:rw \
    //                     -t ghcr.io/zaproxy/zaproxy:stable bash -c \
    //                     "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive_scan.yaml" \
    //                     || true
    //             '''
    //         }
    //         post {
    //             always {
    //                 sh '''
    //                     docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
    //                     docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
    //                     docker stop zap juice-shop
    //                     docker rm zap
    //                 '''
    //                 defectDojoPublisher(artifact: 'results/zap_xml_report.xml', 
    //                 productName: 'Juice Shop', 
    //                 scanType: 'ZAP Scan', 
    //                 engagementName: 'karol.stoinski@gmail.com')
    //             }
    //         }
    //     }
    // }
    }
}
