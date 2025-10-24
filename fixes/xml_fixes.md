# XML Fix Templates

## Automated XML Fixes for Odoo 19

### 1. Tree to List Conversion
```python
# Fix pattern
old_string = '<tree'
new_string = '<list'

old_string = '</tree>'
new_string = '</list>'
```

### 2. Search View Group Removal
```python
import re

def fix_search_views(xml_content):
    # Remove group tags but keep content
    pattern = r'<search>(.*?)<group[^>]*>(.*?)</group>(.*?)</search>'
    replacement = r'<search>\1\2\3</search>'
    return re.sub(pattern, replacement, xml_content, flags=re.DOTALL)
```

### 3. Active ID Replacement
```python
def fix_active_id(xml_content):
    # Replace active_id with id in contexts
    pattern = r'context="([^"]*?)active_id([^"]*?)"'
    replacement = r'context="\1id\2"'
    return re.sub(pattern, replacement, xml_content)
```

### 4. Kanban Template Fix
```python
def fix_kanban_templates(xml_content):
    # Fix template names
    xml_content = xml_content.replace('t-name="kanban-box"', 't-name="card"')

    # Remove js_class
    pattern = r'<kanban([^>]*?)\s+js_class="[^"]*"([^>]*?)>'
    replacement = r'<kanban\1\2>'
    xml_content = re.sub(pattern, replacement, xml_content)

    return xml_content
```

### 5. Cron Job Fix
```python
def fix_cron_jobs(xml_content):
    # Remove numbercall field
    pattern = r'<field name="numbercall"[^>]*>.*?</field>\s*'
    xml_content = re.sub(pattern, '', xml_content, flags=re.DOTALL)

    # Also handle self-closing tags
    pattern = r'<field name="numbercall"[^/]*?/>\s*'
    xml_content = re.sub(pattern, '', xml_content)

    return xml_content
```

### 6. T-if Expression Fix
```python
def fix_t_if_expressions(xml_content):
    # Fix not operator usage
    pattern = r't-if="not\s+(\w+)"'
    replacement = r't-if="\1 == False"'
    return re.sub(pattern, replacement, xml_content)
```

### 7. Complete XML Processor
```python
def process_xml_file(file_path):
    """Apply all XML fixes to a file"""
    with open(file_path, 'r', encoding='utf-8') as f:
        content = f.read()

    # Apply all fixes
    content = content.replace('<tree', '<list')
    content = content.replace('</tree>', '</list>')
    content = content.replace('edit="1"', '')
    content = fix_search_views(content)
    content = fix_active_id(content)
    content = fix_kanban_templates(content)
    content = fix_cron_jobs(content)
    content = fix_t_if_expressions(content)

    # Remove website.snippet_options inheritance
    pattern = r'<template[^>]*inherit_id="website\.snippet_options"[^>]*>.*?</template>\s*'
    content = re.sub(pattern, '', content, flags=re.DOTALL)

    with open(file_path, 'w', encoding='utf-8') as f:
        f.write(content)

    return True
```

## Batch Processing Script
```python
import os
import glob

def fix_all_xml_files(project_path):
    """Process all XML files in a project"""
    xml_files = glob.glob(os.path.join(project_path, '**/*.xml'), recursive=True)

    fixed_count = 0
    errors = []

    for xml_file in xml_files:
        try:
            process_xml_file(xml_file)
            fixed_count += 1
            print(f"✓ Fixed: {xml_file}")
        except Exception as e:
            errors.append((xml_file, str(e)))
            print(f"✗ Error in {xml_file}: {e}")

    print(f"\n✓ Fixed {fixed_count} files")
    if errors:
        print(f"✗ {len(errors)} errors encountered")

    return fixed_count, errors
```

## View-Specific Fixes

### Form Views
```xml
<!-- Before -->
<form string="Title" edit="1">
    <button context="{'default_type_id': active_id}"/>
</form>

<!-- After -->
<form string="Title">
    <button context="{'default_type_id': id}"/>
</form>
```

### List Views
```xml
<!-- Before -->
<tree string="Items" edit="1" editable="top">
    <field name="name"/>
</tree>

<!-- After -->
<list string="Items" editable="top">
    <field name="name"/>
</list>
```

### Kanban Views
```xml
<!-- Before -->
<kanban js_class="custom_kanban">
    <templates>
        <t t-name="kanban-box">
            <div class="oe_kanban_card">
                <!-- content -->
            </div>
        </t>
    </templates>
</kanban>

<!-- After -->
<kanban>
    <templates>
        <t t-name="card">
            <div class="oe_kanban_card">
                <!-- content -->
            </div>
        </t>
    </templates>
</kanban>
```

### Search Views
```xml
<!-- Before -->
<search>
    <field name="name"/>
    <group expand="0" string="Group By">
        <filter name="type" string="Type" context="{'group_by': 'type_id'}"/>
    </group>
    <group expand="0" string="Filters">
        <filter name="active" string="Active" domain="[('active', '=', True)]"/>
    </group>
</search>

<!-- After -->
<search>
    <field name="name"/>
    <filter name="type" string="Type" context="{'group_by': 'type_id'}"/>
    <filter name="active" string="Active" domain="[('active', '=', True)]"/>
</search>
```

## Validation

After applying fixes, validate XML:
```python
from lxml import etree

def validate_xml(file_path):
    """Validate XML syntax"""
    try:
        with open(file_path, 'r', encoding='utf-8') as f:
            etree.parse(f)
        return True, "Valid"
    except etree.XMLSyntaxError as e:
        return False, str(e)
```