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
