properties([
  disableConcurrentBuilds(),
  buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '30'))
])

def platforms = [:]

def packagePlatforms = ["jammy", "noble"]
for (int i = 0; i < packagePlatforms.size(); i++) {
  def platform = packagePlatforms[i]
  platforms["package-$platform"] = { -> node('docker') {
    stage("package-$platform") { timeout(time: 3, unit: 'HOURS') {
      checkout scm
      sh 'git submodule foreach git fetch --tags --force'
      withCredentials([file(credentialsId: 'gpg-sign-key', variable: 'SECRET')]) {
	sh 'cp $SECRET secret.gpg'
      }
      def img = docker.build("flatironinstitute/triqs-package-$platform:${env.BRANCH_NAME}", "-f Dockerfile.package-$platform .")
      img.inside('-v /etc/passwd:/etc/passwd -v /etc/group:/etc/group') {
	sh """#!/bin/bash -ex
	  top=\$PWD
	  for t in triqs triqs_cthyb triqs_ctseg triqs_dft_tools triqs_tprf triqs_maxent triqs_hubbardI triqs_hartree_fock solid_dmft triqs_Nevanlinna ; do
	    mkdir \$top/test/\$t/run
	    cd \$top/test/\$t/run
	    cmake ..
	    make -j2
	    make test CTEST_OUTPUT_ON_FAILURE=1
	  done
	"""
	sh "tar czf ${platform}.tgz -C \$REPO ."
      }
      sh "docker rmi --no-prune ${img.imageName()}"
      archiveArtifacts(artifacts: "${platform}.tgz")
    } }
  } }
}

try {
  parallel platforms
} catch (err) {
  emailext(
    subject: "\$PROJECT_NAME - Build # \$BUILD_NUMBER - FAILED",
    body: """\$PROJECT_NAME - Build # \$BUILD_NUMBER - FAILED

$err

Check console output at \$BUILD_URL to view full results.

Building \$BRANCH_NAME for \$CAUSE
\$JOB_DESCRIPTION

Chages:
\$CHANGES

End of build log:
\${BUILD_LOG,maxLines=60}
    """,
    to: 'nwentzell@flatironinstitute.org, dsimon@flatironinstitute.org',
    recipientProviders: [
    ],
    replyTo: '$DEFAULT_REPLYTO'
  )
  throw err
}
