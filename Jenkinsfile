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

def getBuildCause(job) {
    //Check if the build was triggered by some jenkins user
    def usercause = job.rawBuild.getCause(hudson.model.Cause.UserIdCause.class)
    if (usercause != null) {
        return "UserIdCause"
    }
    
    //Check if the build was triggered by some jenkins project(job)
    def upstreamcause = job.rawBuild.getCause(hudson.model.Cause$UpstreamCause)
    if (upstreamcause != null) {
       return "UpstreamCause"
    }
    
    def branchEventCause = job.rawBuild.getCause(jenkins.branch.BranchEventCause.class)
    if (branchEventCause != null) {
       return "BranchEventCause"
    }
    return "";
       
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
                    def buildCause = getBuildCause(currentBuild)
                    println "the buildCause is == "+ buildCause
                    if (buildCause == "UpstreamCause") {
                        def upstreamBuild = currentBuild.getUpstreamBuilds().last()
                        println "upstreamBuild is == "+ upstreamBuild
                        def changeSet = upstreamBuild.getChangeSets()
                        println "changeSet is == "+ changeSet
                        def logSet = changeSet.get(0)
                        println "logSet is == "+ logSet
                        def changeLog = logSet.getLogs()
                        println "changeLog is == "+ changeLog
                        println "Author Email == "+ changeLog.getAuthorEmail()
                    }
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
