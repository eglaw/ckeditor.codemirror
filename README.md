# CKEditor Codemirror Plugin

This is a mere copy of the [CKEditor Codemirror](https://github.com/w8tcha/CKEditor-CodeMirror-Plugin) made for the only purpose to serve it via `$ composer install` on Packagist.org

All the issues and the questions sould be referred on the original developer.

## Notes about install
Inside your drupal project's **composer.json** there's a little *hack* that needs to be applied, as there's a problem with installation.
You'll write down something like:
```
"require": {
  ...
  "drupal/ckeditor_codemirror": "^1.1",
  "egorlaw/ckeditor_codemirror": "^1.0",
  ...
},
"extras": {
  "installer-paths": {
    "web/core": ["type:drupal-core"],
    "web/libraries/{$name}": ["type:drupal-library"],
    "web/modules/contrib/{$name}": ["type:drupal-module"],
    "web/profiles/contrib/{$name}": ["type:drupal-profile"],
    "web/themes/contrib/{$name}": ["type:drupal-theme"],
  }
}
```

The `$ composer install` comand will do everything quite correct and will place the codemirror plugin inside your library folder as `ckeditor_codemirror`. There's a little problem with that: The contrib module **ckeditor_codemirror** is looking into another path:
`library/ckeditor.codemirror/plugin.js` and unfortunatly it's hardcoded. So to avoid patching the module just make sure to rename the folder during the `$ composer install` by adding this row inside extras.installer-paths:

`"web/libraries/ckeditor.codemirror": ["egorlaw/ckeditor_codemirror"],`

BEFORE the `"web/libraries/{$name}": ["type:drupal-library"],` row!
Resulting in :

```
"extras": {
  "installer-paths": {
    "web/core": ["type:drupal-core"],
    "web/libraries/ckeditor.codemirror": ["egorlaw/ckeditor_codemirror"],   <-- here
    "web/libraries/{$name}": ["type:drupal-library"],
    "web/modules/contrib/{$name}": ["type:drupal-module"],
    "web/profiles/contrib/{$name}": ["type:drupal-profile"],
    "web/themes/contrib/{$name}": ["type:drupal-theme"],
  }
}
```