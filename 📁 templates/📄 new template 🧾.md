---
<% "<%* tR += (await tp.file.include('[[๐ฆ block~yaml]]')).replace(/^---/gm, '').trim(); %>" %>

---

<% "<% await tp.file.include('[[๐ฆ block~header ๐]]') %>" %>

---

<% "<% await tp.file.include('[[๐ฆ block~mentions ๐]]') %>" %>

<%* tR += (await tp.file.include('[[๐ฆ block~meta โ]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>
<%-*
/*

# template

## meta~yaml

```dataviewfield
aliases     :: template
created     :: 2021-07-28T02:45:20-04:00
description :: new template
publish     :: true
requires    :: templater-obsidian
scope       :: 
tags        :: #Obsidian/note/template, #Obsidian/plugin/templater-obsidian
title       :: ๐ template~note~default ๐งพ
type        :: template~note~template
uuid        :: 32ebd3d4-a3cb-4dec-9f95-9d089dd7ff4d
version     :: 1
```

## meta~todo

- [ ] #Obsidian/template/note/template/mobile fix on mobile #TODO

## meta~notes

## meta~inbox

*/
_%>