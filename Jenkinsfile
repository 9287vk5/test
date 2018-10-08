pipeline {
    agent {label 'atf_slave3'} 
    environment {
        THIRD_PARTY_INSTALL_PREFIX="${WORKSPACE}/build/src/3rdparty/LINUX"
        THIRD_PARTY_INSTALL_PREFIX_ARCH="${THIRD_PARTY_INSTALL_PREFIX}/x86"
        LD_LIBRARY_PATH="$THIRD_PARTY_INSTALL_PREFIX_ARCH/lib:$THIRD_PARTY_INSTALL_PREFIX/lib"
    }
    stages {
        stage('Build') { 
            steps {
              git branch: 'test1', url: 'https://github.com/9287vk5/test'
              sh 'printenv'
            }
        }
        stage ('cppcheck') {
            steps {
              sh 'cppcheck --enable=all --inconclusive -i "src/3rd_party-static" -i "src/3rd_party" --xml --xml-version=2 -q src 2> cppcheck.xml'
            }
        }
        stage('Test') { 
            steps {
              echo 'Test' 
            }
        }
        stage('Deploy') { 
            steps {
              echo 'Deploy' 
            }
        }
    }
}
