---
<% "<%* tR += (await tp.file.include('[[📦 block~yaml]]')).replace(/^---/gm, '').trim(); %>" %>

---

<% "<% await tp.file.include('[[📦 block~header]]') %>" %>

---

<% "<% await tp.file.include('[[📁 templates/📦 block~mentions]]') %>" %>

<%* tR += (await tp.file.include('[[📦 block~meta]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>
<%-*
/*

# template

## meta~yaml

```dataviewfielda
aliases     :: template template
created     :: 2021-07-28T02:45:20-04:00
description :: new template
requires    :: Templater
scope       :: 
tags        :: [Obsidian/template]
title       :: 🧾 note~template
type        :: template~note~template
version     :: 1
```

## meta~todo

- [ ] #Obsidian/template/note/template/mobile fix on mobile #TODO

## meta~notes


## meta~inbox

*/
_%>