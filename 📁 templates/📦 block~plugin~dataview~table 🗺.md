```dataview
TABLE regexreplace(row.file.folder, ".*\/([^\/]+)$", "$1") AS "Parent", file.ctime AS "Created", file.mtime AS "Modified"
FROM "<%* tR += tp.file.folder(true).toString().replace(/.*\/([^\/]+)$/, '$1'); %>"
SORT file.mtime DESC
```
<%-*
/*

# template

## meta~data

```dataviewfield
aliases     :: dataview table
created     :: 2021-08-05T15:21:13-04:00
description :: table sorted by recently modified files
requires    :: dataview, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/table, #Obsidian/plugin/dataview, sql, #Obsidian/plugin/templater-obsidian
title       :: 📦 table~dataview 🗺
type        :: template~block~table~plugin~dataview
uuid        :: 51b3279f-aebb-4f6a-ba08-161f9a3cf7de
version     :: 1
```

## meta~todo

## meta~notes

## meta~inbox

*/
_%>