properties([
  disableConcurrentBuilds(),
  buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '30'))
])

def platforms = [:]

def packagePlatforms = ["xenial", "bionic"]
for (int i = 0; i < packagePlatforms.size(); i++) {
  def platform = packagePlatforms[i]
  platforms["package-$platform"] = { -> node('docker') {
    stage("package-$platform") { timeout(time: 1, unit: 'HOURS') {
      checkout scm
      sh 'git submodule foreach git fetch --tags'
      withCredentials([file(credentialsId: 'gpg-sign-key', variable: 'SECRET')]) {
	sh 'cp $SECRET secret.gpg'
      }
      def img = docker.build("flatironinstitute/triqs-package-$platform:${env.BRANCH_NAME}", "-f Dockerfile.package-$platform .")
      img.inside('-v /etc/passwd:/etc/passwd -v /etc/group:/etc/group') {
	sh """#!/bin/bash -ex
	  mkdir test/triqs/run
	  cd test/triqs/run
	  cmake ..
	  make -j2
	  make test
	"""
	sh "tar czf ${platform}.tgz -C \$REPO ."
      }
      sh "docker rmi --no-prune ${img.imageName()}"
      archiveArtifacts(artifacts: "${platform}.tgz")
    } }
  } }
}

parallel platforms
