# Continuous Improvement

[![Documentation Status](https://readthedocs.org/projects/continuous-improvement/badge/?version=latest)](https://continuous-improvement.readthedocs.io/en/latest/?badge=latest)

Documentation can be found at [readthedocs](https://continuous-improvement.readthedocs.io).

## Operating Instructions

### Prerequisites

- Make sure you have
    - Installed Python 3x

### Local Development

- Create and activate a virtual-environment using the following commands
    - ```python -m virtualenv venv```
    - ```source venv/Scripts/activate``` or ```source venv/bin/activate``` depending upon your OS
- Install required python dependencies using ```pip install -r requirements.txt```
- To build docs that are to be deployed using github-pages, use the following command
    - ```make githhub```
- Use ```make html``` to build the docs for other cases
- On successful build, you can view the docs by opening the file ```/build/html/index.html``` in any browser of your choice.
