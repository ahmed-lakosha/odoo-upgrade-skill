# Odoo 18 to 19 Migration Patterns

## Critical Changes

### 1. RPC Service Removal in Frontend
**Impact**: HIGH
**Affected**: All public website components using RPC

The `rpc` service is no longer available in frontend/public components in Odoo 19.

#### Detection
```javascript
grep -r "useService.*['\"]rpc['\"]" --include="*.js"
```

#### Solution
Replace with fetch-based JSON-RPC calls. Add this helper to each affected component:

```javascript
async _jsonRpc(endpoint, params = {}) {
    try {
        const response = await fetch(endpoint, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-Csrf-Token': document.querySelector('meta[name="csrf-token"]')?.content || '',
            },
            body: JSON.stringify({
                jsonrpc: "2.0",
                method: "call",
                params: params,
                id: Math.floor(Math.random() * 1000000)
            })
        });

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        const data = await response.json();
        if (data.error) {
            console.error('JSON-RPC Error:', data.error);
            throw new Error(data.error.message || 'RPC call failed');
        }
        return data.result;
    } catch (error) {
        console.error('JSON-RPC call failed:', error);
        throw error;
    }
}
```

### 2. Kanban View Changes

#### Template Name Change
```xml
<!-- Before -->
<t t-name="kanban-box">

<!-- After -->
<t t-name="card">
```

#### JS Class Removal
```xml
<!-- Before -->
<kanban js_class="crm_kanban">

<!-- After -->
<kanban>
```

### 3. Search View Structure

Group tags are no longer allowed in search views:

```xml
<!-- Before -->
<search>
    <group expand="0" string="Group By">
        <filter name="type" string="Type"/>
        <filter name="status" string="Status"/>
    </group>
</search>

<!-- After -->
<search>
    <filter name="type" string="Type"/>
    <filter name="status" string="Status"/>
</search>
```

### 4. Form View Context Variables

`active_id` is no longer available in form view contexts:

```xml
<!-- Before -->
<button context="{'default_parent_id': active_id}"/>

<!-- After -->
<button context="{'default_parent_id': id}"/>
```

### 5. Cron Job Fields

The `numbercall` field has been removed:

```xml
<!-- Remove this line entirely -->
<field name="numbercall">-1</field>
```

### 6. Website Snippet System

The snippet registration system has changed completely:

```xml
<!-- This no longer works -->
<template id="custom_option" inherit_id="website.snippet_options">
    <!-- content -->
</template>

<!-- New approach: Use the new snippet system or remove -->
```

## Python API Changes

### 1. URL Generation
```python
# Before
from odoo.addons.http_routing.models.ir_http import url_for
url = url_for('/path')

# After
url = self.env['ir.http']._url_for('/path')
```

### 2. Slug Functions
```python
# Before
from odoo.addons.http_routing.models.ir_http import slug, unslug

# After - Add compatibility wrapper
from odoo.http import request

def slug(value):
    return request.env['ir.http']._slug(value)

def unslug(value):
    return request.env['ir.http']._unslug(value)
```

## Theme SCSS Changes

### 1. Color Palette Menu Assignment
```scss
// Must specify menu, footer, copyright colors
$o-color-palettes: map-merge($o-color-palettes, (
    'my_theme': (
        'o-color-1': #207AB7,
        'o-color-2': #FB9F54,
        'o-color-3': #F6F4F0,
        'o-color-4': #ffffff,
        'o-color-5': #191A19,
        'menu': 1,        // NEW: Required
        'footer': 3,      // NEW: Required
        'copyright': 5,   // NEW: Required
    ),
));
```

### 2. Font Configuration Properties
```scss
// Must include properties section
$o-theme-font-configs: map-merge($o-theme-font-configs, (
    'FontName': (
        'family': ('FontName', sans-serif),
        'url': 'FontName:weights&display=swap',
        'properties': (  // NEW: Required
            'base': (
                'font-size-base': 1rem,
                'line-height-base': 1.6,
            ),
        )
    ),
));
```

## JavaScript Module Changes

### 1. Registry Categories
```javascript
// Before
registry.category("public_components").add("name", Component);

// After - May need adjustment based on component type
registry.category("public_components").add("name", Component);
registry.category("actions").add("name", Component);  // For action components
```

### 2. Service Availability
Services available in frontend have changed. Check availability:
- ❌ `rpc` - Not available
- ✅ `localization` - Available
- ✅ `bus` - Available (via env.bus)

## Testing After Migration

### Critical Tests
1. All JavaScript components load without console errors
2. RPC calls return data correctly
3. Kanban views render properly
4. Search filters work
5. Forms submit correctly
6. Theme displays with correct colors
7. Cron jobs execute

### Commands
```bash
# Clear assets and regenerate
rm -rf filestore/assets
python -m odoo -d [DB] --dev=xml,css,js

# Test module installation
python -m odoo -d test_db -i [module] --stop-after-init

# Check for JavaScript errors
# Open browser console and look for errors
```

## Rollback Plan

If migration fails:
1. Restore from backup
2. Document specific error
3. Apply targeted fix
4. Re-run migration
5. Test thoroughly

## Known Issues

1. **MapTiler/External Services**: May need API key updates
2. **Custom Widgets**: May need complete rewrite
3. **Report Templates**: Check PDF generation
4. **Email Templates**: Verify rendering