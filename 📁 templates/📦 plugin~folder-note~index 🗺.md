---
<%*
if (tp.file.path(false).match(/Error_MobileUnsupportedTemplate/i)) { 
    const notice = `${tp.config.template_file.name} is not supported on mobile.`;
    new Notice(notice, 2000);
    return;
}
const timerLabel = `${tp.file.path(true)}:templater.generate@${moment().format('YYYYMMDDHHmmss')}`;
console.time(timerLabel);
const folder = tp.file.folder(false) !== '' ? tp.file.folder(false) : this.app.vault.adapter.getName();
const folderPath = (tp.file.folder(true) ?? tp.file.path(false).replace(/\\/g, '/')) ?? '/';
const matchEmoji = /[^\x00-\x7F]+/g;
const folderAlias = `${folder?.replace(matchEmoji, '').trim()} ${folder?.match(matchEmoji)?.join('') ?? ''}`.trim();
tR += (await tp.file.include('[[📦 block~yaml ✉]]'))
    // remove yaml directives
    .replace(/^---/gm, '')
    // replace aliases
    .replace(/^((?:[ ]+)?aliases(?:[ ]+)?:)(?:[ ]+)?(.*)$/gm,
        `$1 [${'$2, ' ?? ''}"${folderAlias} overview 🗺", "overview 🗺 of ${folderAlias}", "${folderAlias} table ⛓", "table ⛓ of ${folderAlias}", "${folder}"]`)            
    // replace tags
    .replace(/^((?:[ ]+)?tags(?:[ ]+)?:)(?:[ ]+)?$/gm,
        `$1 [Obsidian/plugin/folder-note-plugin, overview, index, table]`)
    // preserve existing uuid
    .replace(/^((?:[ ]+)?uuid(?:[ ]+)?:)(?:[ ]+)?(.*)$/gm,
        `$1 ${tp.frontmatter?.uuid ? `&uuid ${tp.frontmatter?.uuid}` : '$2'}`)
    // replace metadata.description
    .replace(/^((?:[ ]+)?description(?:[ ]+)?:)(?:[ ]+)?$/gm,
        `$1 "files and file properties within [\`${folderPath}\`](` + encodeURI(`file:///${tp.file.folder(true).replace(/\\/g, '/')}`) + ')"')
    // replace metadata.title
    .replace(/^((?:[ ]+)?title(?:[ ]+)?:)(?:[ ]+)?.*$/gm,
        `$1 🗺 Overview of ${folder}`)
    // replace metadata.type
    .replace(/^((?:[ ]+)?type(?:[ ]+)?:)(?:[ ]+)?$/gm,
        `$1 overview~table`)
    .trim();

%>

---

<% await tp.file.include('[[📦 block~header 🔝]]') %>

---

<% await tp.file.include("[[📦 block~plugin~dataview~js~table ⛓]]") %>

<%* tR += (await tp.file.include("[[📦 block~mentions 🔙]]")).replace(new RegExp('-file:\\(.*\\)', 'ig'), `-path:(${folderPath})`).replace(new RegExp(tp.file.title, 'ig'), folder); %>

## Inbox

<%-*
//
/*

# template

## meta~data

```dataviewfield
aliases     :: folder-note
created     :: 2021-08-05T22:54:34-04:00
description :: [[software~Obsidian~plugin~folder-note-plugin|Folder Note]] template
mobile      :: false
publish     :: true
requires    :: folder-note-plugin, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/type/x
title       :: 📦 folder-note~index 🗺
type        :: template~note~plugin~folder-note
uuid        :: a5c3b511-00d8-40c9-8b58-bd872001ade9
version     :: 2
```

## meta~todo

- [ ] #Obsidian/template/plugin/folder-note find command to open directly to a plugin's options page #TODO

## meta~notes

## meta~inbox

*/
console.timeEnd(timerLabel);
_%>