#!/usr/bin/env groovy
def get_last_tag(repo_name){
    println("Get Last Tag of $repo_name")
    str_list = sh("git describe --abbrev=0 --tags").text.trim().split('\\.')
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
    def final_tag = "${new_tag[0]}.${new_tag[1]}.${new_tag[2]}"
    println "Create Local Tag $final_tag"
    def command = "git tag -a $final_tag -m \"Tag: $final_tag for $repo_name\""
    sh(command)
    println "Push tag $final_tag on $remote_name"
    command = "git push $remote_name $final_tag"
    sh(command)
}


def remote_name = "origin"
def repo_name = "Helloworld"

//list_update = update_tag(get_last_tag(repo_name))
//create_tag(list_update, remote_name, repo_name)


pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                script {
                    list_update = update_tag(get_last_tag(repo_name))
                    create_tag(list_update, remote_name, repo_name)
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


