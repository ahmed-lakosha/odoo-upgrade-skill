![GitHub release](https://img.shields.io/github/v/release/ahmed-lakosha/odoo-upgrade-skill)
![GitHub stars](https://img.shields.io/github/stars/ahmed-lakosha/odoo-upgrade-skill)
![License](https://img.shields.io/badge/license-LGPL--3.0-blue)

# Odoo Upgrade Skill for Claude Code

A comprehensive Claude Code skill for automating Odoo ERP module upgrades between versions 14-19, with special focus on the breaking changes in Odoo 19.

## 🚀 Features

- **Automated Pattern Detection**: Identifies and fixes common migration issues
- **Multi-Version Support**: Handles migrations from Odoo 14 through 19
- **RPC Service Migration**: Automatically converts frontend RPC calls for Odoo 19
- **XML Transformation**: Fixes view definitions, kanban templates, and search views
- **Python API Updates**: Handles import changes and deprecated methods
- **Theme Migration**: Updates SCSS variables and font configurations
- **Comprehensive Error Catalog**: Documents 25+ common errors with solutions
- **Helper Scripts**: Python scripts for batch processing

## 📦 Installation

### As a Claude Code Skill

1. Copy the skill to your Claude Code skills directory:
```bash
# For project-specific use
cp -r C:\tmp\plugins\odoo-upgrade-skill .claude\skills\

# For global use
cp -r C:\tmp\plugins\odoo-upgrade-skill %USERPROFILE%\.claude\skills\
```

2. The skill will be automatically available when you ask Claude to upgrade Odoo modules.

### As a Standalone Tool

1. Clone or copy the repository:
```bash
git clone <repository-url> odoo-upgrade-skill
cd odoo-upgrade-skill
```

2. Install Python dependencies:
```bash
pip install lxml
```

## 🎯 Quick Start

### Using with Claude Code

Simply ask Claude:
- "Upgrade my Odoo module from version 17 to 19"
- "Fix RPC service errors in my Odoo 19 module"
- "Migrate my theme to Odoo 19"

Claude will automatically use this skill when detecting Odoo upgrade tasks.

### Using Helper Scripts Standalone

#### Fix RPC Service Issues
```bash
python scripts/fix_rpc_service.py path/to/module
```

#### Update Manifests
```bash
python scripts/upgrade_manifest.py path/to/module --target 19
```

#### Process Entire Project
```bash
python scripts/upgrade_manifest.py path/to/project --recursive --target 19
python scripts/fix_rpc_service.py path/to/project
```

## 📋 What Gets Fixed

### JavaScript/Frontend (Odoo 19)
- ✅ RPC service removal and replacement with fetch
- ✅ Module annotations (`/** @odoo-module **/`)
- ✅ Service registration changes
- ✅ Import path updates

### XML Views
- ✅ `<tree>` → `<list>` conversion
- ✅ Remove `edit="1"` attributes
- ✅ Fix search view `<group>` tags
- ✅ Replace `active_id` with `id`
- ✅ Kanban template name changes (`kanban-box` → `card`)
- ✅ Remove `js_class` attributes
- ✅ Remove `numbercall` from cron jobs

### Python Code
- ✅ `slug` function compatibility
- ✅ `url_for` import changes
- ✅ External dependency declarations
- ✅ API decorator updates

### Themes/SCSS
- ✅ Variable naming conventions
- ✅ Font configuration with `map-merge`
- ✅ Color palette menu/footer assignments
- ✅ Unit conversions (px → rem)

### Manifests
- ✅ Version format (e.g., `19.0.1.0.0`)
- ✅ Missing license key
- ✅ External dependencies
- ✅ Auto-detect Python packages

## 🔍 Example: Relief Center Migration

This skill was developed while migrating a complex humanitarian aid system from Odoo 17 to 19:

**Project Stats:**
- 5 interdependent modules
- 115 files analyzed
- 32 files modified
- 450+ lines changed
- 10 RPC calls migrated
- 7 JavaScript components fixed

**Key Issues Resolved:**
1. Frontend RPC service unavailable
2. Kanban views broken
3. Search filters not working
4. Theme colors not applying
5. MapTiler integration failing

## 📚 Documentation Structure

```
odoo-upgrade-skill/
├── SKILL.md                    # Main skill definition
├── patterns/
│   ├── common_patterns.md      # Universal patterns
│   └── odoo18_to_19.md        # Version-specific changes
├── fixes/
│   ├── xml_fixes.md            # XML transformation templates
│   └── javascript_fixes.md     # JS/OWL migration templates
├── scripts/
│   ├── upgrade_manifest.py     # Manifest updater
│   └── fix_rpc_service.py      # RPC service fixer
├── reference/
│   └── error_catalog.md        # 25+ common errors
└── README.md                    # This file
```

## 🛠️ Manual Intervention Required

Some issues require manual review:
- Complex business logic changes
- Custom widget rewrites
- Third-party module compatibility
- Database schema migrations
- Report template updates

## 🧪 Testing After Upgrade

Always test after upgrading:

```bash
# Install upgraded module
python -m odoo -d test_db -i module_name --stop-after-init

# Run with development mode
python -m odoo -d test_db --dev=xml,css,js

# Run tests
python -m odoo -d test_db --test-enable -i module_name
```

## 🔄 Version Compatibility

| From Version | To Version | Difficulty | Major Changes |
|--------------|------------|------------|---------------|
| 17 → 18 | 18 | Low | Minor API changes |
| 18 → 19 | 19 | **High** | RPC removal, view changes |
| 17 → 19 | 19 | **Very High** | Complete frontend rewrite |
| 16 → 17 | 17 | Medium | OWL framework adoption |

## 🚨 Common Pitfalls

1. **Not backing up** before upgrade
2. **Upgrading all modules at once** instead of incrementally
3. **Ignoring external dependencies** in manifests
4. **Not clearing asset cache** after changes
5. **Missing theme color assignments** (menu, footer)

## 🤝 Contributing

To improve this skill:

1. Document new error patterns
2. Add fix templates for new issues
3. Update version-specific guides
4. Share migration experiences

## 📄 License

LGPL-3.0 (Compatible with Odoo licensing)

## 🙏 Acknowledgments

Developed during the successful migration of the Relief Center humanitarian aid system, processing real-world production code with complex interdependencies.

## 📞 Support

For issues or improvements:
- Create an issue in the repository
- Submit pull requests with new patterns
- Share your migration experiences

## 🎯 Pro Tips

1. **Always upgrade in a test environment first**
2. **Use version control** - commit before and after each major change
3. **Test incrementally** - one module at a time
4. **Document custom changes** that the skill can't handle
5. **Keep the skill updated** as new Odoo versions are released

---

*Built with real-world experience from production Odoo migrations*## 🚀 Quick Install

```bash
# Clone the skill
git clone https://github.com/ahmed-lakosha/odoo-upgrade-skill.git

# Copy to Claude skills directory (Windows)
xcopy /E /I odoo-upgrade-skill %USERPROFILE%\.claude\skills\odoo-upgrade-skill

# Or for Linux/Mac
cp -r odoo-upgrade-skill ~/.claude/skills/
```
