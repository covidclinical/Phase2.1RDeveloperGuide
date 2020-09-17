# Phase2.1RDeveloperGuide
4CE is an international consortium for electronic health record (EHR) data-driven studies of the COVID-19 pandemic. The goal of this effort—led by the i2b2 international academics users group—is to inform doctors, epidemiologists and the public about COVID-19 patients with data acquired through the health care process.

You can learn more about the consortium, the data it is collecting, its members, and how to participate here:

https://covidclinical.net

This document is intended to give an overview of the software development guidelines for participaing sites that intend to author their own analytic tools and workflows.

# Overview: Federated Data Analyses

The original work of the consortium, described in an [August, 2020 Nature Digital Medicine article](https://www.nature.com/articles/s41746-020-00308-0), relied on individual clinical sites reporting aggregate data (case counts by day, summary lab measurement values, demographic breakdowns, etc.) about their COVID-19 patient population back to a single team for analysis. This approach had the obvious limitation that individual patient-level variables (comorbidities, medication histories, etc.) were not available for analysis. To enable analysis of such data without requiring that patient-level records be shared among institutions, we are introducing a federated analysis model.  

Under this model, each site will run patient-level analyses locally on their own data, and report back the site's results for meta-analysis by the project lead.  All consortium members may propose analyses for which they would like to serve as a project lead.  If the proposal is approved by the 4CE steering committee, the project lead will be responsible for writing the software, as an R package, to implement the analytic workflow on the 4CE standardized data model. That software will be distributed to the sites, will run locally on the sites' patient-level data, and will contain functionality to automatically perform validation of the analysis' results and push the results (after user inspection) to a project-specific data repository. With that repository serving as a central collection point, the project lead can then perform a meta-analysis of the site-level results.

The remainder of this document will provide an overview (with references to additional details) of the data model, the computational environment in which the analytic workflows will be executed, and guidelines for writing software in R to implemenet those workflows.

# 4CE Data Extraction and Standarized Data Model
## TODO: Griffin, others, please write a paragraph or a few here (or put them in a README.md on the appropriate GitHub repository and provide a link) to give an overview of the process and motivation for the standardized data model.  Please make sure to include something like this as a catch-all for sites that are doing their own data engineering:

Those sites that do not implement an i2b2 instance that is compatible with the provided ETL process can still participate by creating a custom extraction routine against their own backend data warehouse, as long as the output conforms to the 4CE schema.

# 4CE Computational Environment

In order to create a consistent, homogenous computing environment where the analytic packages can be run independently at each of the participating sites, we are distributing a Docker image, which we will refer to as the 4CE Container.  The 4CE Container contains a pre-built environment that has been configured with all of the tools necessary to bootstrap individual analytic workflows in R. 

The containter comes pre-configured with both command-line R, as well as RStudio Server. It runs both sshd and rstudio-server on startup. Users and developers can interact with R either by using RStudio Server in a browser, or by ssh'ing to the container and running R on the command line.  See documentation for the container (linked below) for more details.

The container is intended to provide developers a consistent base layer for their software. It is *not* intended to contain every dependency required for every workflow. Analytic workflows that have dependencies that are not already available on the 4CE Container should declare those dependencies through the "Imports" section of "DESCRIPTION" file for the package that implements the workflow.  See: Software Engineering Guidelines below.

A pre-built image of the 4CE Container is available to pull from DockerHub:

https://hub.docker.com/repository/docker/dbmi/4ce-analysis/general

And the respective Dockerfile is available on GitHub:

https://github.com/covidclinical/Phase2.1DockerAnalysis/blob/master/Dockerfile

For additional details on working with the Docker container, including instructions for building the image on a bastion host and moving to an isolated host where the analyses will be run, see the [README](https://github.com/covidclinical/Phase2.1DockerAnalysis) in the GitHub repository for the container definition.

An up-to-date (as of this writing) listing of the R packages available in the 4CE Container can be found in the container's GitHub repository:

https://github.com/covidclinical/Phase2.1DockerAnalysis/blob/master/R_VERSIONS

A current listing of all of the pre-installed R packages can be generated by executing `installed.packages()` in an R session in an instance of a running 4CE container.

# 4CE Software Engineering Guidelines

Pointers to R reference material
-creating packages
-handling dependencies

General best practices:
Camel-casing
Master as production branching

# Fixed-interval "run all"

# TODO: A lot of this material should be scraped out and replicated in a "Site User Guide", and incorporate instructions for running the individual analyses or checkpointed containers