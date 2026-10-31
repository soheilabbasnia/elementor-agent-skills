# WP-CLI Commands Reference for Elementor

Essential WP-CLI commands and workflows for managing Elementor sites:

## Elementor Official CLI Commands
If the official Elementor WP-CLI commands are loaded:
```bash
# Clear compiled CSS cache
wp elementor flush-css

# Sync remote template library
wp elementor library sync

# Migrate Elementor database after plugin updates
wp elementor update db

# Replace URLs across Elementor content
wp elementor replace-urls <old-url> <new-url>
```

## Listing Elementor Pages & Templates
```bash
# List all pages built with Elementor
wp post list --post_type=page --meta_key=_elementor_edit_mode --meta_value=builder \
  --fields=ID,post_title,post_name,post_status

# List saved Elementor templates in the library
wp post list --post_type=elementor_library --fields=ID,post_title,post_status

# List global widgets
wp post list --post_type=elementor_library --meta_key=_elementor_template_type \
  --meta_value=widget --fields=ID,post_title
```

## Emergency Recovery
If a page breaks or Elementor fails to load in safe mode:
```bash
# Export postmeta to inspect corruption
wp post meta get <POST_ID> _elementor_data > debug.json

# Restore from a previous backup
wp post meta update <POST_ID> _elementor_data "$(cat /tmp/elementor-backup.json)"
wp elementor flush-css
```
