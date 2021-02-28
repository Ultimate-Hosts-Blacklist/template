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
            sh script: 'conda activate ${BUILD_TAG}', label: 'Activate Environment'
            sh script: 'python -V', label: 'Get Python Version'
            sh script: 'pip --version', label: 'Get pip version'
            sh script: 'pip install ultimate-hosts-blacklist-test-launcher', label: 'Install Launcher'
            sh script: 'PyFunceble --version', label: 'Get PyFunceble version'
            sh script: 'ultimate-hosts-blacklist-test-launcher --version', label: 'Get Launcher version'
            sh script: 'conda deactivate', label: 'Deactivate conda environment'
          }
      }
    }

    post {
        always {
            sh script:'conda remove --yes -n ${BUILD_TAG} --all', label: 'Destroy conda environment'
        }
    }
}
