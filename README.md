# Testing framework for Digital Documentation of COVID-19 Certificates (DDCC)

Read this in [English](README.md), [française](README.fr.md), [中文](README.zh.md), [русский](README.ru.md), [español](README.es.md), [العربية](README.ar.md)

This repository documents the test steps for the DDCC Implementation Guide (IG) and also and emphasizes the methodology for new learners about using FHIR-oriented artifacts.

## Component Versions

The DDCC IG is tested against the latest release versions of the primary components in the workflow. This holds the other components relatively constant and focuses on differences of the main branch of DDCC, rather than testing against different versions of tooling. Components should be tested in their own repositories. The objective is to test the DDCC IG itself and not against not a matrix of versions of components.

| Component | Version/Container | Notes, Todo |
| --- | --- | --- |
| Ubuntu (ubuntu-latest) | **20.04** | GitHub Actions (nektos/act image is different)  |
| DDCC IG | **head of main branch** | All runs use the latest main branch |
| DDCC Transactions Mediator | **head of main branch** | All runs use the latest main branch |
| FHIR | **4.0.1** | Todo: matrix against new FHIR releases |
| Node | **16.x** LTS | |
| Jekyll | **4.2.0** | |
| Sushi | **2.0.1** | Will be updated due to frequent bug fixes |
| Java | **11.x** | Eclipse Temurin (formerly AdoptOpenJDK) |
| Publisher | **1.1.77** | Will be updated due to frequent bug fixes |
| Kubernetes | **1.21.1** | Uses [kind](https://github.com/kubernetes-sigs/kind/releases) default, Todo: Use Instant's default | 
| Instant OpenHIE (core) | **0.0.5** |
| HAPI FHIR Server | **5.4.1** |  |
| OpenHIM Console | **1.14** |  |
| OpenHIM API Server | **openhim-core:7** |  |

## Workflows

### ![build ddcc fsh](https://github.com/intrahealth/instant-openhie-ddcc/workflows/build_fsh/badge.svg) Build FSH using Sushi

Many IGs use FHIR Shorthand (FSH) to create conformance resources and examples. DDCC uses FSH. As a test, Sushi (aka "SUSHI Unshortens Short Hand Inputs") is run to generate JSON files from FSH files. Running this step indicates if there are any outstanding IG issues from the FSH files. This is helpful to do before running Publisher.

### ![build ddcc ig](https://github.com/intrahealth/instant-openhie-ddcc/workflows/build_ig/badge.svg) Build IG using Publisher

The `build_ig` workflow builds the DDCC Implementation Guide and outputs an artifact for the QA report. This is a different workflow than publishing an IG to GitHub Pages, for which DDCC is an [example](DDCC-ghpages).

[Publisher](https://github.com/HL7/fhir-ig-publisher) creates a full IG output including HTML pages using the Ruby-based Jekyll engine. A helpful output from Publisher is the QA report `qa.html`. This is captured during the workflow and provided as an artifact on the workflow page. When Publisher runs and the IG is published, the QA report is linked on the footer of the pages.


### WIP: ![confirm instant docker core](https://github.com/intrahealth/instant-openhie-ddcc/workflows/confirm_coredocker/badge.svg) Confirm Instant Core Package on Docker Compose

> WIP

### ![submit health event](https://github.com/intrahealth/instant-openhie-ddcc/workflows/submithealthevent/badge.svg) Submit Health Event to DDCC Transactions Mediator

Artifact: PNG of QR Code

## How to Run Locally

This repository has been specially designed to ensure that all of the GitHub Actions on your local computer and can be used to reproduce all results. This is accomplished using [nektos/act](https://github.com/nektos/act) ("Run your GitHub Actions locally 🚀"). The Actions include building IGs, testing transactions using [Kubernetes-in-Docker](https://github.com/kubernetes-sigs/kind/) and other Actions. To run locally, follow the instructions to install `act`. The .actrc has been kept in the repository for reference.

When act is used, some features of GitHub Actions may not work as expected. This is often because act uses the Docker engine on a platform, and not a full virtual machine as GitHub Actions does. So, some commands, like kubectl, are already included in all GitHub Actions runners but not in act. To address this, act includes an environment variable `env.ACT` which indicates if the workflows are run locally or on the Microsoft Azure VM infrastructure that GitHub uses.

```
# Run this step only if ACT is being used.
- name: Some step
if: ${{ env.ACT }}
# Or, run this step only if ACT is not being used.
if: ${{ !env.ACT }}
```


## Translation of this README

An example of how to translate such READMEs is in the Jupyter (iPython) notebook `translate_readme.ipynb`.
