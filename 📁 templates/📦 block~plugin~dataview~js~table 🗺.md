```dataviewjs
dv.table(
    [// headings
        //'Titles',
        //'CSS Class',
        //'Publish',
        //'Tags',
        //'Breadcrumbs',
        //'UUID',
        'Created',
        'Modified',
        //'Datetime',
        'Description',
        'Scope',
        //'Status',
        'Title',
        'Type',
        //'Source',
        //'Requires',
        //'Version',
        //'File',
        //'Outlinks',
        //'Inlinks',
        //'Tasks',
        //'file.tasks.text',
        //'file.tasks.complete',
        //'file.tasks.fullyComplete',
        //'file.tasks.real',
        //'file.tasks.path',
        //'file.tasks.line',
        //'file.tasks.subtasks',
    ],
    // filter
    dv.pages('')
    .where(n =>  n.file.folder === dv.current().file.folder)
    //.where(n =>  n.file.name !== dv.current().file.name)
    // sort
    .sort(n => n.file.mtime.toString(), 'desc')
    // columns
    .map(n => {
        return [
    // Titles
        //Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // CSS Class
        //n?.cssclass,
    // Publish
        //n?.publish,
    // Tags
        //Array.from(new Set(n.file.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),
        //{
    // Breadcrumbs
        //    Container   : n?.container,
        //    Near        : n?.alongside,
        //    Contains    : n?.contains,
        //},
    // UUID
        //n?.uuid ? `\`${n?.uuid}\`` : '\\-',
    // Created
        moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n.file.ctime.toString()).format('YYYYMMDD[T]HH:mm'),
    // Modified
        moment(n.file.mtime.toString()).format('YYYYMMDD[T]HH:mm'),
    // Datetime
        //{
        //    Julian  : n?.metadata?.datetime.julian,
        //    CDay    : moment(n.file.cday.toString()).format('YYYY-MM-DD'),
        //    MDay    : moment(n.file.mday.toString()).format('YYYY-MM-DD'),
        //    
        //},
    // Description
        n?.metadata?.description ?? n?.description,
    // Scope
        n?.metadata?.scope ?? n?.scope,
    // Status
        //{
        //    Archived: n?.metadata?.status.archived ? (n?.metadata?.status.archived ? "✔" : "❌") : "✖",
        //    Reviewed: n?.metadata?.status.reviewed ? (n?.metadata?.status.reviewed ? "✔" : "❌") : "✖",
        //},
    // Title
        n?.title ? `[[${n.file.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n.file.path}|${n?.metadata?.title}]]` : n.file.link),
    // Type
        n?.metadata?.type ? n?.metadata?.type : (
            n?.type ? n?.type : (n.file.name.match(/~/) ? 
            n.file.name.replace(/[^\x00-\x7F]+/g, '').trim().split('~')[0] : 
            n.file.name.replace(/[^\x00-\x7F]+/g, '').trim().replace(/new/ig, ''))
        ),
    // Source
        //{
        //    Target  : n?.source?.target,
        //    Template: n?.source?.template,
        //    Version : n?.source?.version,
        //},
    // Requires
        //n?.requires?.split(',').map(v => {
        //    let r = this.app.plugins.getPlugin(v.trim())?.manifest.name;
        //    return `[[software~Obsidian~plugin~${v.trim()}|${r === undefined ? v.trim() : r}]]`;
        //}),
    // Version
        //n?.version ?? n?.source?.version,
    // File
        //{
        //    Path    : `[[${n.file.path}|${n.file.path}]]`,
        //    Folder  : `[[${n.file.folder}/🗺 index.md|${n.file.folder}]]`,
        //    Size    : n.file.size,
        //    Ext     : n.file.ext,
        //},
    // Outlinks
        //n.file.outlinks,
    // Inlinks
        //n.file.inlinks,
    // Tasks
        //n.file.tasks,
        //dv.array(n.file.tasks).text,
        //dv.array(n.file.tasks).complete,
        //dv.array(n.file.tasks).fullyComplete,
        //dv.array(n.file.tasks).real,
        //dv.array(n.file.tasks).path,
        //dv.array(n.file.tasks).line,
        //dv.array(n.file.tasks).subtasks,
    ]})
    // limit
    .slice(0, 50)
);
```
<%-*
/*

# template

## meta~data

```dataviewfield
aliases     :: dataviewjs table
created     :: 2021-08-05T06:06:39-04:00
description :: dataviewjs table with current template fields
requires    :: dataview, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/table, #Obsidian/plugin/dataview/js, javascript, #Obsidian/plugin/templater-obsidian
title       :: 📦 table~dataview~js 🗺
type        :: template~block~table~plugin~dataview~js
uuid        :: 85a26ae5-9e0a-4c49-ac63-51a06bbc24b2
version     :: 1
```

## meta~todo

## meta~notes

## meta~inbox

*/
_%>