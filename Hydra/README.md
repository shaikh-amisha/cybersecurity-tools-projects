# Hydra Labs

This folder contains hands-on exercises using THC Hydra for understanding installation, verification, and controlled authentication testing.

## Labs

1. [Lab 01: Install and Verify Hydra](Lab-01-Install-and-Verify-Hydra.md)
   - Install Hydra using the system package manager
   - Verify the executable location
   - Run a controlled SSH password-list test against localhost

2. [Lab 02: Crack a Specific User Account](Lab-02-Crack-a-Specific-User-Account.md)
   - Use a supplied password list against the `securityadmin` account
   - Configure Hydra's `http-post-form` module for a local login page
   - Specify the target port, form fields, and failed-login response
   - Save and verify the result in `hydra_results.txt`

Each write-up documents the objective, environment, commands, command explanations, observed results, and key learnings.

## Responsible Use

Hydra performs authentication attempts. Use it only against systems you own or have explicit permission to test. The practical exercises documented here target controlled local LabEx environments.
