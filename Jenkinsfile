def who_is_upstream(job) {
    def run = job.rawBuild.getCause(hudson.model.Cause$UpstreamCause)?.getUpstreamRun()
    if (run == null) {
        return null
    }
    return run
}
def recursive_upstream(job) {
    def upstream = who_is_upstream(job)
    if(upstream != null)
    {
        upstream = recursive_upstream(upstream)
    }
    return job
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Get Upstream') {
            steps {
                script {
                    echo 'Getting upstream'
                    def upstreamJob = recursive_upstream(currentBuild)
                    println "upsteam Job ="+upstreamJob
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
