---
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid 6996b8c2-93b5-483c-a193-ac24b5f4c2e8
# built-in [[software~Obsidian#yaml]]
aliases : *uuid
cssclass: 
publish : 
tags    : [library for templates, template library, template settings, template regex, template functions, template reference, template dependency]
# for [[software~Obsidian~plugin~breadcrumbs]]
conatiner   : 
near        : 
contains    : 
# this note
metadata:
    datetime: 
        created : 2021-08-07T02:08:14-04:00
        # from [[software~Obsidian~plugin~obsidian-juliandate]]
        julian  : 2459433.58906
    description : dependency for other template files. contains reusable strings and regex
    scope       : 
    status      :
        archived: false
        reviewed: false
    title       : 📑 library~templates
    type        : 
# this note's template
source:
    publish : 
  # target  : "[[📁 templates/📑 library~settings.md]]"
    template: "[[📁 templates/📦 block~yaml ✉.md]]"
    version : 1
settings:
    datetime:
        formatColumn: 'YYYY-MM-DD HH:mm'
        formatDay   : 'YYYY-MM-DD'
regex:
    matchEmoji          : /[^\x00-\x7F]+/g
    matchPathParent     : /(?:(?:\/)?(?<parent>[^\/]+))?(?:(?:\/)?(?<target>(?:[^\/]+))(?:\/)?)$/
    

---

# `=this.metadata.title`

**`=this.metadata.description`**, modified: _`$=moment(dv.current().file.mtime.toString()).format('LLLL')`_

---

```ad-info

This note is intended as a reference for other files. It should not be included in other templates via `tp.file.include()` or inserted into notes as a template.

```

```ad-danger

Changing non-standard values for fields within this note can break templates. Proceed with caution.

```