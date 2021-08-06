---
<%_*
let metadata                = {};
metadata.datetime           = {};
metadata.datetime.created   = moment().format();
metadata.datetime.julian    = this.app.plugins?.getPlugin('obsidian-juliandate')?.computeJulianDay();
metadata.scope              = this.app.vault.getName().replace(/[^\x00-\x7F]+/g,'').toLowerCase().trim();
metadata.uuid               = ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16));
%>
<%-* if (this.app.plugins?.getPlugin('obsidian-advanced-uri')?._loaded) { %>
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid <% metadata.uuid %>
<%-* } %>
# built-in [[software~Obsidian#yaml]]
aliases : *uuid
cssclass: 
publish : 
tags    : 
<%-* if (this.app.plugins?.getPlugin('breadcrumbs')?._loaded) { %>
# for [[software~Obsidian~plugin~breadcrumbs]]
container   : 
near        : 
contains    : 
<%-* } %>
# this note
metadata:
    datetime: 
        created : <% metadata.datetime.created %>
        # from [[software~Obsidian~plugin~obsidian-juliandate]]
<%-* if (this.app.plugins?.getPlugin('obsidian-juliandate')?._loaded) { %>
        julian  : <% metadata.datetime?.julian %>
<%-* } %>
    description : 
    scope       : <% metadata.scope %>
    status      :
        archived: false
        reviewed: false
    title       : <% tp.file.title %>
    type        : 
# this note's template
source:
  # target  : "[[<% tp.config.target_file.path %>]]"
    template: "[[<% tp.config.template_file.path %>]]"
    version : <% tp.config.template_file.stat.mtime %>

---
<%_ *
/* 
# template

## meta~data

```dataviewfield
aliases     :: yaml
created     :: 2021-08-05T16:37:53-04:00
description :: yaml containing [[software~Obsidian#yaml|built-in]], [[softwarw~Obsidian~plugin~obsidian-advanced-uri|Advanced Obsidian URI]], [[software~Obsidian~plugin~breadcrumbs|Breadcrumbs]], and [[software~Obsidian~plugin~obsidian-juliandate|Julian Date]] fields
requires    :: breadcrumbs, obsidian-advanced-uri, obsidian-juliandate, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/yaml, #yaml, #Obsidian/plugin/breadcrumbs, #Obsidian/plugin/obsidian-advanced-uri, #Obsidian/plugin/obsidian-juliandate, #Obsidian/plugin/templater-obsidian
title       :: 
type        :: template~block~yaml
uuid        :: 758ef8c6-7034-49d8-b9c1-8e64de713eb0
version     :: 1
```

## meta~todo

- [ ] #Obsidian/template/block/yaml create a loop for other templates to preserve existing frontmatter data when overwriting a file - see yaml `uuid` replace operation within [[📄 new uri 🔗]] for example #TODO

## meta~notes

## meta~inbox

*/
_%>