[![Python application test with Github Actions](https://github.com/noahgift/gcp-from-zero/actions/workflows/python-publish.yml/badge.svg)](https://github.com/noahgift/gcp-from-zero/actions/workflows/python-publish.yml)

# gcp-from-zero
This repository is a fork of the original project by Noah Gift. It's designed to guide you through setting up and deploying projects on Google Cloud Platform (GCP), utilizing Cloud Build for CI/CD workflows.

## First thing to do is DevOps Workflow

* Get SSH-Keys setup in a Cloud Environment

`ssh-keygen -t ed25519 -C "yourmail"`

Give github this content: `cat ~/.ssh/id_ed25519.pub`

Then checkout the code.
* Python Scaffold

1. Create virtualenv

`virtualenv --python $(which python) ~/.venv`

2.  Source it

`source ~/.venv/bin/activate`


* Setup Continuous Integration

## To integrate Cloud Build instead of Github Actions

Example here:  [cloudbuild.yaml](https://github.com/noahgift/gcp-from-zero/blob/main/cloudbuild.yaml)

```
steps:
- name: python:3.9
  id: INSTALL
  entrypoint: python3
  args:
  - '-m'
  - 'pip'
  - 'install'
  - '-t'
  - '.'
  - '-r'
  - 'requirements.txt'
- name: python:3.9
  entrypoint: ./pylint_runner
  id: LINT
  waitFor:
  - INSTALL
- name: "gcr.io/cloud-builders/gcloud"
  args: ["app", "deploy"]
timeout: "1600s"
images: ['gcr.io/$PROJECT_ID/pylint']
```
