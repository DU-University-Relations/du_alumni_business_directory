# DU Alumni Business Directory

This package provides the Alumni Owned Business content type, Business
Categories vocabulary, supporting fields and displays, the Alumni Business
Directory View, and module-owned templates and styling. It is intended for the
DU Alumni downstream rather than the shared DU upstream.

## Installation

Install the package with Composer and enable `du_alumni_business_directory`.
The package type is `drupal-custom-module-package`, so DU Composer projects
install it under `web/modules/packages`.

The module supports Drupal 10 and 11. Its default configuration requires the
File, Image, Link, Menu UI, Node, Path, SVG Image, Taxonomy, Text, User, and
Views modules. The DU upstream currently supplies the contributed SVG Image
dependency.

On a fresh module installation, Drupal imports the files under
`config/install`. The install hook then creates the standard Business
Categories terms when they do not already exist.

## Runtime behavior

The module provides full and teaser node templates and attaches the
`alumni-business` library from those templates. It also changes the exposed
`field_region_value` element into a select list populated from distinct stored
region values.

The form alter currently applies to every Views exposed form that contains a
`field_region_value` element. This preserves the behavior of the D10 Alumni
site; narrowing it to the Alumni Business Directory View requires downstream
behavioral verification.

## Maintenance and verification

This package was reconciled with the checked-in D10 Alumni implementation and
then processed with the DU PHP 8.3 and Drupal 10 Rector rules used for Drupal
11 preparation. For a release, verify at minimum:

- Composer installation into `web/modules/packages`
- module enablement and cache rebuild on supported Drupal 10 and 11 sites
- the `/alumni-business-directory` View and exposed filters
- full and teaser rendering for an Alumni Owned Business node
- preservation of active downstream fields, displays, and View configuration

## Release workflow

This package is published through Packagist rather than Drupal.org, so the
version in `du_alumni_business_directory.info.yml` is maintained manually.
Packagist derives the Composer package version from the Git tag; do not add a
`version` field to `composer.json`.

For each release:

1. Create a release branch from `main`. The info file should contain the
   intended release with a `-dev` suffix, such as `9.25.3-dev`.
2. Complete the release changes and run the documented verification checks.
3. Remove the `-dev` suffix in the info file, using a quoted value such as
   `version: '9.25.3'`, and commit the release version.
4. Tag that commit with the matching Git tag, such as `v9.25.3`, and push the
   release branch and tag. Packagist will publish the version represented by
   the tag.
5. Confirm that the new version and expected commit appear on Packagist.
6. Merge the release branch back into `main`, update the info file to the next
   planned development version, such as `version: '9.25.4-dev'`, and commit the
   post-release version bump.

The release tag and the version in the tagged info file must match. Do not
leave `main` on an exact released version; it should identify the next planned
development version with the `-dev` suffix.

## Known follow-up work

- Add focused coverage for the region exposed-filter behavior. The current
  procedural hook and database query make this better suited to a kernel or
  functional test than a low-cost unit test.
- Review whether the broad region form alter should target only the Alumni
  Business Directory View.
- Review the explicit uninstall cleanup before changing it. Uninstall currently
  deletes the module's content type, vocabulary, field configuration, and View.
- Decide whether optional Simple XML Sitemap and URL Redirect form components
  in the exported form display should remain in reusable default configuration.
- Add explicit Composer constraints for Drupal core and SVG Image after the
  package dependency policy and supported version ranges are confirmed.
- Confirm and add the intended package license before the next release.
