# TRIQS Packaging

This builds the [packages](https://users.flatironinstitute.org/~ccq/triqs/) in Jenkins for TRIQS and selected applications and runs tests.

## Dependencies

The submodules in this repository point to the latest released revision of each module, and are updated automatically at the end of each successful Jenkins run for master or unstable, but only for tagged revisions on release branches.

## Tests

Tests to run on installed packages must be symlinked into the test directory from the appropriate submodules.
