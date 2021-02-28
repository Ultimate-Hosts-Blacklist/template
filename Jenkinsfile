pipeline {
    agent any

    triggers {
        cron('H */4 * * *')
    }

    environment {
        GIT_NAME = credentials('uhb-git-name')
        GIT_EMAIL = credentials('uhb-git-email')
        GITHUB_TOKEN = credentials('uhb-pat')
        GIT_REPO_NAME = env.GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')
    }

    stages {
      stage('Setup miniconda') {
        steps {
          sh script: '''#!/usr/bin/env bash
          source ${UHB_CONDA_DIR}/etc/profile.d/conda.sh
          conda create --yes -n ${BUILD_TAG} python=3.9 pip
          ''', label: 'Setup Conda environment'
        }
      }

      stage('Run') {
          steps {
            sh script: '''#!/usr/bin/env bash
            source ${UHB_CONDA_DIR}/etc/profile.d/conda.sh

            conda activate ${BUILD_TAG}
            python -V
            pip --version
            pip install ultimate-hosts-blacklist-test-launcher
            PyFunceble --version
            ultimate-hosts-blacklist-test-launcher --version

            conda deactivate
            '''
          }
      }
    }

    post {
        always {
            sh script: '''#!/usr/bin/env bash
            source ${UHB_CONDA_DIR}/etc/profile.d/conda.sh

            conda remove --yes -n ${BUILD_TAG} --all
            '''
        }
    }
}
