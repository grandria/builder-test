#!/usr/bin/env groovy
def get_last_tag(repo_name){
    println("Get Last Tag of $repo_name")
    str_list = sh(script: "git describe --abbrev=0 --tags", returnStdout: true).trim().split('\\.')
    int_list = []
    for (items in str_list) {
        int_list.add(items.toInteger())
    }
    println "last tag is $int_list"
    int_list
}

def update_tag(version_list){
    if (version_list[2] < 9) {
        version_list[2]++
        if (version_list[2] >= 9) {
            version_list[2] = 0
            version_list[1]++
            if (version_list[1] >= 9) {
                version_list[1] = 0
                version_list[0]++
            }
        }
    } else if (version_list[2] >= 9) {
        version_list[2] = 0
        version_list[1]++
        if (version_list[1] >= 9) {
            version_list[1] = 0
            version_list[0]++
        }
    }
    version_list
}

def create_tag(new_tag, remote_name, repo_name){
    sh("git config --global user.name \"grandria\"")
    sh("git config --global user.email \"grandrianarivony@alkemics.com\"")
    def final_tag = "${new_tag[0]}.${new_tag[1]}.${new_tag[2]}"
    println "Create Local Tag $final_tag"
    def command = "git tag -a $final_tag -m \"Tag: $final_tag for $repo_name\""
    sh(command)
    try {
        println "Push tag $final_tag on $remote_name:master"
        command = "git push $remote_name $final_tag:master"
        sshagent(credentials: ['grandria-git-key']){
            sh(command)
        }
    } catch(Exception e) {
        error "Pushing new tags"
    }
}


def remote_name = "origin"
def repo_name = "Helloworld"

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                script {
                    create_tag(update_tag(get_last_tag(repo_name)), remote_name, repo_name)
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}


