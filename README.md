# Memory Leak Detection with Docker and GitHub Actions

This repository demonstrates an automated approach to detect memory leaks in Docker containers using a GitHub Actions workflow. It leverages the `scripts/checkForMemoryLeaks.ts` script that checks for memory leaks in your code, which is run as part of an end-to-end (E2E) testing pipeline using GitHub Actions.

## Overview

Detecting memory leaks in software applications is a significant challenge in software development. Memory leaks can degrade the performance of an application over time and can lead to system crashes. To tackle this issue, this repository provides a GitHub Actions workflow that utilizes Docker containers and a custom Node.js script for automated memory leak detection during the E2E testing process.

## How It Works

1. **`scripts/checkForMemoryLeaks.ts` Script**: This Node.js script checks the memory usage of a Docker container before and after the E2E tests. If the final memory usage exceeds a certain threshold (50% by default), the script flags a potential memory leak and halts the process. The script takes three command-line arguments: the Docker container's name, its initial memory usage, and its final memory usage.

2. **`.github/workflows/e2e-tests.yml` Workflow**: This GitHub Actions workflow orchestrates the process. It records the initial memory usage of the Docker containers, runs the E2E tests, and then records the final memory usage. These values are then fed to the `scripts/checkForMemoryLeaks.ts` script for analysis.

The workflow also prepares and uploads the logs of all containers as an artifact. This facilitates debugging and helps in identifying issues during the E2E tests.

## Getting Started

To use this workflow in your project, follow these steps:

1. Install Docker and GitHub CLI on your local machine.
2. Clone this repository: `gh repo clone <repository-url>`.
3. Navigate into the repository's directory: `cd <repository-name>`.
4. Adjust the script and workflow files according to your project's specific needs.
5. Push the changes to your repository.

Remember to replace the container names in the `CONTAINERS_TO_MEM_CHECK` and `CONTAINERS_TO_LOG` environment variables in the `.github/workflows/e2e-tests.yml` file with the names of your own Docker containers.

## Limitations

Although this solution provides an automated approach to detect memory leaks, it does have certain limitations:

- **Possible False Alarms**: The script might produce false positives (flagging normal memory usage increases as leaks) or false negatives (missing actual memory leaks).
- **Memory Threshold Balance**: The threshold for memory usage increase should be carefully chosen. A high threshold could miss minor leaks, whereas a low threshold might lead to frequent false alarms.
- **Variations in Memory Management**: Memory management in Docker containers can vary depending on the technology stack of your application, which might affect the accuracy of this solution.

## Further Reading

For a detailed guide on how this setup works and how to integrate it into your own projects, check my blog post [Automating Memory Leak Detection: Improve Your Dockerized CI Pipelines Now](https://lachiejames.com/automating-memory-leak-detection-improve-your-dockerized-ci-pipelines-now/). The post explains each step of the process and provides additional context and explanation.
