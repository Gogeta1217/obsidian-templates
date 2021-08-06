---
<%_*
const bc = {};
if (this.app.plugins?.getPlugin('breadcrumbs')?._loaded) {
    const vaultTabSize = this.app.vault.config.tabSize;
    function padKey(k) {
        r = k.length % vaultTabSize;
        return r === 0 ? k : k + " ".repeat(vaultTabSize - r);
    }
    bc.p = padKey(this.app.plugins.getPlugin('breadcrumbs').settings.parentFieldName);
    bc.s = padKey(this.app.plugins.getPlugin('breadcrumbs').settings.siblingFieldName);
    bc.c = padKey(this.app.plugins.getPlugin('breadcrumbs').settings.childFieldName);
    while (bc.p.length !== bc.s.length && bc.s.length !== bc.c.length && bc.c.length !== bc.p.length)
    {
        while (bc.p.length > bc.s.length) {
            bc.s = bc.s.padEnd(bc.s.length + vaultTabSize);
        }
        while (bc.s.length > bc.c.length) {
            bc.c = bc.c.padEnd(bc.c.length + vaultTabSize);
        }
        while (bc.c.length > bc.p.length) {
            bc.p = bc.p.padEnd(bc.p.length + vaultTabSize);
        }
    }
}
const metadata = {};
metadata.datetime = {};
metadata.datetime.created = moment().format();
metadata.datetime.julian = this.app.plugins?.getPlugin('obsidian-juliandate')?.computeJulianDay();
metadata.scope = this.app.vault.getName().replace(/[^\x00-\x7F]+/g,'').toLowerCase().trim();
metadata.uuid = ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16));
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
<%* tR += bc.p; %>: 
<%* tR += bc.s; %>: 
<%* tR += bc.c; %>: 
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
    publish : false
  # target  : <%* tR += `"[[${tp.config.target_file.path}]]"` %>
    template: <%* tR += `"[[${tp.config.template_file.path}]]"` %>
    version : <%* tR += `${this.app.plugins.getPlugin('dataview')?.api?.page(tp.config.template_file.path)?.version ?? tp.config.template_file.stat.mtime}` %>

---
<%_ *
/* 
# template

## meta~data

```dataviewfield
aliases     :: yaml
created     :: 2021-08-05T16:37:53-04:00
description :: yaml containing [[software~Obsidian#yaml|built-in]], [[softwarw~Obsidian~plugin~obsidian-advanced-uri|Advanced Obsidian URI]], [[software~Obsidian~plugin~breadcrumbs|Breadcrumbs]], and [[software~Obsidian~plugin~obsidian-juliandate|Julian Date]] fields
publish     :: true
requires    :: breadcrumbs, dataview, obsidian-advanced-uri, obsidian-juliandate, templater-obsidian
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