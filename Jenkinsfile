#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage("build") {
            steps {
                retry(3) { echo "Build" }
            }
        }
        stage("tests") {
            steps {
                parallel (
                    "unit test" : {
                        build("unit-test-job")
                    },
                    "component test" : {
                        build("component-test-job")
                    }
                )
            }
        }
    }
}
