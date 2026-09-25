# Release 1.3.0

## Clean-release changes

- Standardized package version to 1.3.0.
- Added explicit maintainer metadata: **@yantrrstudio**.
- Replaced placeholder licensing with the MIT License and preserved attribution.
- Added `NOTICE.md` for project and third-party attribution guidance.
- Added package-hygiene rules to prevent audit output and runtime debris from shipping.
- Removed generated audit artifacts and sample runtime artifacts from the distributable.
- Removed generated Emoji source output from the skill package; it remains a consuming-project artifact/template.
- Registered `WORKFLOW_AUDIT.md` in the manifest.
- Updated tests for the release version and complete agent registry.
- Added a root README describing execution modes, gates, and release hygiene.
