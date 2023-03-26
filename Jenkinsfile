pipeline {
    agent any

    stages {
        stage('Configure'){
            steps {
                sh '''
                rm -rf build;
                mkdir -p build;
                cd build;
                cmake -G "Unix Makefiles" \
                      -DCMAKE_INSTALL_PREFIX=./install \
                      -DCMAKE_BUILD_TYPE=Debug \
                      ..
               '''
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh '''
                   cd build
                   make -j \
                   '''

            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh '''
                   cd build
                   pwd
                   ls -l
                   ctest -R test_uarm
                   '''
            }
        }
    }
}
