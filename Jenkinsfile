def who_caused(build) {
    def buildUser = build.rawBuild.getCause(hudson.model.Cause$UserIdCause)
    if (!buildUser) {
        return null 
    }
    [
        id: buildUser.getUserId(),
        name: buildUser.getUserName(),
        description: buildUser.getShortDescription()
    ]   
}

def who_is_upstream(job) {
    def upstreamcause = job.rawBuild.getCause(hudson.model.Cause.UpstreamCause.class)
    
    println "upstreamcause ==> "+ upstreamcause
    if (upstreamcause != null) {
        upstreamJob = Jenkins.getInstance().getItemByFullName(upstreamcause.getUpstreamProject(), hudson.model.Job.class)
        println "this is the upstream Job "+ upstreamJob
        if (upstreamJob != null) {
            println "upstreamcause.getUpstreamBuild()== "+ upstreamcause.getUpstreamBuild()
            upstream = upstreamJob.getBuildByNumber(upstreamcause.getUpstreamBuild())
            if (upstream != null) {
                println "this is the upstream now == "+ upstream
                return upstream
            }
            //println "there is no upstream sorry it is null"
            return upstreamJob
        }
        println "there is no upstreamJob sorry it is null"
        return null
    }
    if (upstreamcause == null) {
        return null
    }
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
                    println "current build == "+currentBuild
                    println "upstream builds for currentBuild == "+ currentBuild.getUpstreamBuilds()
                    for(upstreamBuild in currentBuild.getUpstreamBuilds()){
                       println "upstreamBuild URL == "+ upstreamBuild.getAbsoluteUrl()
                       println "upstreamBuild getDisplayName() == +"upstreamBuild.getDisplayName()
                       println "upstreamBuild changeset == "+ upstreamBuild.getChangeSets()
                    }
                    //def upstreamJob = recursive_upstream(currentBuild)
                    //println "the upsteram Job == "+upstreamJob
                    //def who_caused_this = who_caused(currentBuild)
                    //println "who_caused_this Job failure ="+who_caused_this
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
