pipeline {
    agent {label 'atf_slave3'} 
    environment {
        THIRD_PARTY_INSTALL_PREFIX="${WORKSPACE}/build/src/3rdparty/LINUX"
        THIRD_PARTY_INSTALL_PREFIX_ARCH="${THIRD_PARTY_INSTALL_PREFIX}/x86"
        LD_LIBRARY_PATH="$THIRD_PARTY_INSTALL_PREFIX_ARCH/lib:$THIRD_PARTY_INSTALL_PREFIX/lib"
        GIT_BRANCH="$BRANCH"
    }
    stages {
        stage ('Preparation') {
            steps {
                sh '''
                    ulimit -c unlimited;
                    rm -rf /tmp/corefiles;
                    mkdir /tmp/corefiles;
                    echo '/tmp/corefiles/core.%e.%p' | sudo tee /proc/sys/kernel/core_pattern;
                    GIT_COMMIT="$(git rev-parse --short HEAD)";
                '''
                echo 'Desc: "SDL: " ${GIT_BRANCH:0:29} " " ${GIT_COMMIT:0:8}'
            }
        }/*
        stage ('check-style') {
            steps { sh 'tools/infrastructure/check_style.sh || true' }
        }*/
        stage ('cppcheck') {
            steps { sh 'cppcheck --enable=all --inconclusive -i "src/3rd_party-static" -i "src/3rd_party" --xml --xml-version=2 -q src 2> cppcheck.xml' }
        }/*
        stage ('cmake') {
            steps {
                dir('build') {
                    sh 'cmake .. -DBUILD_TESTS=ON'
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
        stage ('UT') {
            steps { dir('build') {
                sh '''
                    make test | tee ut.log || true; result=${PIPESTATUS[0]};
                    if [ $result -ne 0 ]; then
                     COREFILE=$(find /tmp/corefiles -type f -name 'core*');
                     echo $COREFILE;
                     grep -w "SegFault" ut.log | while read -r line; do 
                      arr=($line); 
                      echo ${arr[3]};
                     done > res.txt;
                     test_file=$(find ${WORKSPACE}/build/src/components/ -type f -name "$(cat res.txt)");
                     echo $test_file;
                     echo "Started gdb!";
                     echo thread apply all bt | gdb $test_file $COREFILE;
                     tar -zcf coredump.tar.gz /tmp/corefiles/
                     pwd
                     find ${WORKSPACE}/build/src/components/ -maxdepth 3 -mindepth 3 -type f -executable| xargs tar -uf tests.tar
                     gzip -c tests.tar > tests.tar.gz
                     tar -zcf OpenSDL.tar.gz bin/
                     exit 2;
                    fi
                '''
            }}
        }
        stage ('tar') {
            steps {
                sh '''
                    cp -r src/3rdparty/LINUX/x86/lib/* bin
                    cp -r src/3rdparty/LINUX/lib/* bin
                    mkdir bin/api
                    cp -r ../src/components/interfaces/* bin/api
                    cp CMakeCache.txt bin
                    tar -zcf OpenSDL.tar.gz bin
                '''
            }
        }*/}
        post {
            always {
                publishCppcheck pattern:'cppcheck.xml'
        }
    }
}
