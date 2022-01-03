pipeline {
  agent any

  environment {
    
    WORKSPACE = '.'
    DBRKS_BEARER_TOKEN = "xyz"
    DBTOKEN="DBTOKEN"
    CLUSTERID="1228-220746-bqqkddxs"
    DBURL="adb-6840195589605290.10.azuredatabricks.net"
  }

  stages {
    stage('Install Miniconda') {
        steps {
            sh '''#!/usr/bin/env bash
            echo "Inicianddo os trabalhos"  
            wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -nv -O miniconda.sh

            rm -r $WORKSPACE/miniconda
            bash miniconda.sh -b -p $WORKSPACE/miniconda
            
            export PATH="$WORKSPACE/miniconda/bin:$PATH"
            
            echo $PATH

            conda config --set always_yes yes --set changeps1 no
            conda update -q conda
            conda create --name mlops

            '''
        }

    }

     stage('Configure Databricks') {
        steps {
           withCredentials([string(credentialsId: DBTOKEN, variable: 'TOKEN')]) { 
            sh """#!/bin/bash
                
                source $WORKSPACE/miniconda/etc/profile.d/conda.sh
                conda activate miniconda/envs/mlops/

                # Configure Databricks CLI for deployment
                echo "${DBURL} $TOKEN" | databricks configure --token

                # Configure Databricks Connect for testing
                echo "${DBURL} $TOKEN ${CLUSTERID} 0 15001" | databricks-connect configure
              """
           }
      }
    }
  } 
}