# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [v1.0.0] - 2026-08-24

### Changed

- The pull request commenter has changed and now uses the `borchero/terraform-plan-comment` action.
  
  This change is incompatible with any in-flight pull requests. The older comments will not be updated, and new comments from `` will be posted.``

## [v0.18.0] - 2026-08-06 

**Deprecation notice:** The legacy `terraform-reusable.yml` workflow was removed in `v0.18.0` (August 2026), following a deprecation notice to ADOES in June 2026 and confirmation that no repository in the NIT-Administrative-Systems organization still referenced it. Use `tofu-reusable.yml` instead.

If you need the legacy workflow while migrating, pin to `@v0.17.0` and see [Migration from Terraform to OpenTofu](#migration-from-terraform-to-opentofu) below.