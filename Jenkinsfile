pipeline {
  agent any

  environment {
    DB_HOST = 'NAG1-LHP-N89103\\SQLPOC'
    DB_PORT = '1433'
    DB_NAME = 'mirror'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/vineet9925/Python-new-project.git'
      }
    }

    stage('Run SQL Scripts') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'mssql-creds', usernameVariable: 'DB_USER', passwordVariable: 'DB_PASS')]) {
          bat """
            @echo off
            setlocal enabledelayedexpansion
            for %%f in (sql\\*.sql) do (
              echo Running: %%f
              sqlcmd -S %DB_HOST%,%DB_PORT% -d %DB_NAME% -U %DB_USER% -P %DB_PASS% -b -i "%%f"
              if errorlevel 1 exit /b 1
            )
          """
        }
      }
    }
  }

  post {
    success { echo '✅ SQL scripts applied successfully.' }
    failure { echo '❌ SQL script execution failed — check console log.' }
  }
}
