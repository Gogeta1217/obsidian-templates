---
<% "<%* tR += (await tp.file.include('[[📦 block~yaml]]')).replace(/^---/gm, '').trim(); %>" %>

---

<% "<% await tp.file.include('[[📦 block~header 🔝]]') %>" %>

---

<% "<% await tp.file.include('[[📦 block~mentions 🔙]]') %>" %>

<%* tR += (await tp.file.include('[[📦 block~meta ✉]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>
<%-*
/*

# template

## meta~yaml

```dataviewfield
aliases     :: template
created     :: 2021-07-28T02:45:20-04:00
description :: new template
requires    :: templater-obsidian
scope       :: 
tags        :: #Obsidian/note/template, #Obsidian/plugin/templater-obsidian
title       :: 📄 template~note~default 🧾
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