def call(body) {
body()
node('server_build_slave') {
try {
properties([
    parameters([
        string(name: 'SITE_URL', defaultValue: 'http://germaniumhq.com/',
                description: 'URL to test the accesibility against')
    ])
])

SITE_URL = params.SITE_URL ?: 'http://germaniumhq.com/'

stage('Test URL') {
    node {
        deleteDir()
        checkout scm

        docker.build('germaniumhq/pa11y')
            .inside {
            sh """
                pa11y -c /pa11y.config.json "${SITE_URL}"
            """
        }
    }
}
catch (err) {
    currentBuild.result = "FAILURE"
    throw err
 }

}
}
}

