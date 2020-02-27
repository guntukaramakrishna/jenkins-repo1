def who_is_upstream(job) {
    def run = job.rawBuild.getCause(hudson.model.Cause$UpstreamCause)?.getUpstreamRun()
    println "who_is_upstream ==>"+run
    if (run == null) {
        return null
    }
    return run
}
def recursive_upstream(job) {
    def upstream = who_is_upstream(job)
    if(upstream != null)
    {
        println "recursive_upstream going ==>"+upstream
        upstream = recursive_upstream(upstream)
        println "recursive_upstream back ==>"+upstream
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
                    println "current build "+currentBuild
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
