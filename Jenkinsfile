pipeline {
    agent {label 'atf_slave3'} 
    environment {
        THIRD_PARTY_INSTALL_PREFIX="${WORKSPACE}/build/src/3rdparty/LINUX"
        THIRD_PARTY_INSTALL_PREFIX_ARCH="${THIRD_PARTY_INSTALL_PREFIX}/x86"
        LD_LIBRARY_PATH="$THIRD_PARTY_INSTALL_PREFIX_ARCH/lib:$THIRD_PARTY_INSTALL_PREFIX/lib"
    }
    stages {
        stage ('check-style') {
            steps { 
              sh 'tools/infrastructure/check_style.sh || true'
            }
        }
        stage ('cppcheck') {
            steps {
              sh 'cppcheck --enable=all --inconclusive -i "src/3rd_party-static" -i "src/3rd_party" --xml --xml-version=2 -q src 2> cppcheck.xml'
            }
        }
        stage ('cppcheck publish') {
            steps {
              cppcheckPublisher pattern: 'cppcheck.xml'
            }
        }
        /*
        stage ('cmake') {
            steps {
                dir('build') {
                    sh 'cmake .. -DBUILD_TESTS=OFF'
                }
            }
        }
        stage ('build') {
            steps {
                dir('build') {
                    sh '''
                    make install-3rd_party_logger
                    make install -j4
                    '''
                }
            }
        }
*/
    }
}
