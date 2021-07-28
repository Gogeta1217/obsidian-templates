---
scope: 
type: folder
title: 🗺 Index of 📁 templates
description: auto-generated index
aliases: ["📁 templates🗺 index"]
tags: [plugins/folder-note, index]
timestamp:
    created:
        zone: -04:00
        second: 22
        minute: 26
        hour: 1
        day:
            week: 6
            month: 10
            quarter: 9
            year: 191
        week: 28
        month: 7
        quarter: 3
        year: 2021
        unix: 1625894782571

---

# `=this.title`
__`=this.description`__, modified: *<%+ tp.file.last_modified_date() %>*

---

## Overview

```dataviewjs
let templateFolder = new RegExp(app['plugins']['plugins']["templater-obsidian"]['settings']['template_folder']) || new RegExp("templates");
dv.table(["Created", "Modified", "Type", "Title", "Description"], dv.pages("")
    .where(n =>  n.file.folder === dv.current().file.folder)
    .where(n =>  n.file.name !== dv.current().file.name)
    .sort(n => n.file.mtime.toString(), 'desc')
    .map(n => [ moment(n.file.ctime.toString()).format('YYYYMMDD[T]HH:mm'), moment(n.file.mtime.toString()).format('YYYYMMDD[T]HH:mm'), (n.type ? n.type : (n.file.folder.match(templateFolder) ? "template" : (n.file.name.match(/~/) ? n.file.name.split('~')[0] : "\\-"))), (n.title ? '[[' + n.file.folder + "/" + n.file.name + '|' + n.title + ']]' : n.file.link), n.description]));
```

## Mentions 📁 templates

```query
"📁 templates" -path:(📁 templates/)
```
