## Latest : v0.9.16

## v0.9.16 – Update to CARLA 0.9.16

### Major changes

*   Updated all CARLA references to 0.9.16, including the minimum supported version and bundled assets.
*   Rebuilt Docker image on Ubuntu 24.04 with Python 3.12 and streamlined PythonAPI dependency installation.
*   Refreshed README to highlight CARLOS integration and the maintained Docker workflow for this fork.

## v0.9.15 – Initial Release

### Major changes

*   Created GitHub workflow and Dockerfile to automatically build Docker images
*   Update to [CARLA 0.9.15](https://carla.org/2023/11/10/release-0.9.15/)
*   Update to Ubuntu 22.04 and Python 3.10 including corresponding pip versions

### Minor changes

*   Small fix related to CARLA autopilot
*   Modified `scenario_runner.py` to return the result of scenario execution as exit code
