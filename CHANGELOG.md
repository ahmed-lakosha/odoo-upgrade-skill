# Changelog

All notable changes to the Odoo Module Upgrade Skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-10-23

### Added
- Initial release of Odoo Module Upgrade Skill
- Support for Odoo versions 14, 15, 16, 17, 18, 19
- Multi-version upgrade paths with cumulative changes
- Automatic Python code transformations:
  - `name_get()` → `_compute_display_name()`
  - Hook signature updates
  - Context key migrations
  - Access control method updates
- Automatic XML/View transformations:
  - `attrs={}` → direct expressions
  - `<tree>` → `<list>` conversion
  - Settings view restructuring
  - `states={}` removal
- JavaScript/OWL migrations:
  - Legacy widget → OWL v1/v2 components
  - ES6 module conversions
  - publicWidget implementations
- Theme upgrade support:
  - Bootstrap 3/4 → Bootstrap 5
  - LESS → SCSS conversion
  - Modern utility class updates
- Safety features:
  - Automatic backups
  - Test database validation
  - Comprehensive error handling
- Reporting:
  - Detailed upgrade reports
  - Change logs
  - Manual steps documentation
  - Deployment checklists
- Tool integration:
  - `odoo-bin upgrade_code` execution
  - Test suite running
  - Installation validation

### Features
- 100+ transformation patterns
- Version-specific change detection
- Intelligent upgrade path calculation
- Rollback instructions
- Enterprise module compatibility checks

### Documentation
- Comprehensive README with badges
- Installation instructions
- Usage examples
- Troubleshooting guide
- Learning resources

## [Unreleased]

### Planned
- GUI progress indicator
- Batch module upgrades
- Custom transformation rules
- Pre-upgrade validation checks
- Post-upgrade optimization suggestions
- Integration with OpenUpgrade
- Database migration coordination
- Automated testing framework
- Performance benchmarking
- Module dependency graph visualization

---

## Version History

- **1.0.0** (2025-10-23): Initial marketplace release