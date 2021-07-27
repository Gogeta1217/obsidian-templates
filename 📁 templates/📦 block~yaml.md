---
<%_*
let metadata                = {};
metadata.datetime           = {};
metadata.datetime.created   = moment().format();
metadata.datetime.julian    = this.app.plugins?.getPlugin('obsidian-juliandate')?.computeJulianDay();
metadata.scope              = this.app.vault.getName().replace(/[^\x00-\x7F]+/g,'').toLowerCase().trim();
metadata.uuid               = ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16));
%>
# built-in [[obsidian~yaml]]
aliases : 
cssclass: 
publish : 
tags    : 
<%-* if (this.app.plugins?.getPlugin('breadcrumbs')?._loaded) { %>
# for [[obsidian~plugin~breadcrumbs]]
container   : 
alongside   : 
contains    : 
<%-* } %>
# for [[obsidian~plugin~obsidian-advanced-uri]]
uuid        : <%* tR += metadata.uuid; %>
# this note
metadata:
    datetime    : 
        created : <%* tR += metadata.datetime.created; %>
        # from [[obsidian~plugin~obsidian-juliandate]]
<%-* if (this.app.plugins?.getPlugin('obsidian-juliandate')?._loaded) { %>
        julian  : <%* tR += metadata.datetime.julian ?? ''; %>
<%-* } %>
    description : 
    scope       : <%* tR += metadata.scope; %>
    status      :
        archived: false
        reviewed: false
    title       : <% tp.file.title %>
    type        : 
# this note's template
source  :
  # target  : "[[<% tp.config.target_file.path %>]]"
    template: "[[<% tp.config.template_file.path %>]]"
    version : <% tp.config.template_file.stat.mtime %>

---
<%_ *
/* 
# meta

## yaml

```
aliases     :: yaml block template
description :: yaml block for all template files
requires    :: [obsidian-juliandate, Templater]
scope       :: 
tags        :: [Obsidian/template/block/yaml, yaml, Obsidian/plugin/Templater, Obsidian/plugin/obsidian-juliandate]
title       :: yaml template
type        :: template~block~yaml
```

## todo

- [ ] create a loop for other templates to preserve existing frontmatter data when overwriting a file - see yaml `uuid` replace operation within [[📄 new uri 🔗]] for example

## notes

## inbox

*/
_%>