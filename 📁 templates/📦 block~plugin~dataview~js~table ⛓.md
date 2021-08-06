## `fas:Map` Table

```dataviewjs
// Variables
const format = 'YYYYMMDD[T]HH:mm';
const formatDay = 'YYYY-MM-DD'
const matchEmoji = /[^\x00-\x7F]+/g;
const folderNoteName = this.app.plugins.getPlugin('folder-note-plugin')?.settings?.folderNoteName;
const breadcrumbs = this.app.plugins.getPlugin('breadcrumbs')?.settings;
// Table
dv.table(
    [// headings
        'Publish',
        'Created',
        'Modified',
        'Scope',
        'Parent',
        'Title',
        //'Aliases',
        //'Names',
        'Description',
        'Type',
        //'Tags',
        //'UUID',
        //'Version',
        'Links',
        'Task',
        //'Breadcrumbs',
        //'CSS Class',
        //'Datetime',
        //'File',
        //'file.tasks.text',
        //'file.tasks.complete',
        //'file.tasks.fullyComplete',
        //'file.tasks.real',
        //'file.tasks.path',
        //'file.tasks.line',
        //'file.tasks.subtasks',
        //'Inlinks',
        //'Outlinks',
        //'Requires',
        //'Source',
        //'Status',
    ],
    // filter
    dv.pages('')
    .where(n =>  n.file.folder.match('^' + dv.current().file.folder))
    //.where(n =>  n.file.name !== dv.current().file.name)
    // sort
    .sort(n => n.file.mtime.toString(), 'desc')
    // columns
    .map(n => {
    // Functions
        String.prototype.tooltip = function(s) {
            return `<span role='img' aria-label='${s}'>${this}</span>`
        };
    // Variables
        const folderNote = `${n.file.folder}/${folderNoteName}.md`;
        return [
    // Publish
        (n?.publish ?? n?.source?.publish) ? ((n?.publish ?? n?.source?.publish) ? '✔' : '❌') : null,
    // Created
        moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n.file.ctime.toString()).format(format),
    // Modified
        moment(n.file.mtime.toString()).format(format),
    // Scope
        n?.metadata?.scope ?? n?.scope,
    // Parent
        `[${n.file.folder.replace(/.*\/([^\/]+)$/, '$1')}](` + (app.vault.getAbstractFileByPath(folderNote) ? encodeURI(folderNote) : encodeURI(`file:///${app.vault.adapter.getFullPath(n.file.folder.replace(/\\/g, '/'))}`)) + ')',
    // Title
        n?.title ? `[[${n.file.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n.file.path}|${n?.metadata?.title}]]` : n.file.link),
    // Aliases
        //Array.from(new Set(n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Names
        //Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Description
        n?.metadata?.description ?? n?.description,
    // Type
        n?.metadata?.type ? n?.metadata?.type : (
            n?.type ? n?.type : (n.file.name.match(/~/) ? 
            n.file.name.replace(matchEmoji, '').trim().split('~')[0] : 
            n.file.name.replace(matchEmoji, '').trim().replace(/new/ig, ''))
        ),
    // Tags
        //Array.from(new Set(n.file.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),
    // UUID
        //n?.uuid ? `\`${n?.uuid}\`` : null,
    // Version
        //n?.version ?? n?.source?.version,
    // Links   
        (n.file.outlinks?.length === 0 && n.file.inlinks?.length === 0) ? '❌'.tooltip('orphan') : `${ n.file.inlinks.length > 0 ? '🔽'.tooltip('backlinks') + ' ' + n.file.inlinks.length : ''} ${ n.file.outlinks.length > 0 ? '🔼'.tooltip('outlinks') + ' ' + n.file.outlinks.length : ''}`.trim(),
    // Task
        Array.from(new Set(dv.array(n.file.tasks).map(v => {
            let m = v.text.match(/(?<t>\#)/i)?.groups?.t.toLowerCase(); 
            return m !== undefined ? `${m}/${v.completed}` : null
        }))).filter(v => v != null).sort().map(v => { return v.match('true') ? '✔' : '❌' }).sort(v => v, 'desc')[0] ?? null,
    // Breadcrumbs
        //{
        //    [breadcrumbs.parentFieldName]   : n[breadcrumbs.parentFieldName],
        //    [breadcrumbs.siblingFieldName]  : n[breadcrumbs.siblingFieldName],
        //    [breadcrumbs.childFieldName]    : n[breadcrumbs.breadcrumbs],
        //},
    // CSS Class
        //n?.cssclass,
    // Datetime
        //{
        //    Julian  : n?.metadata?.datetime.julian,
        //    CDay    : moment(n.file.cday.toString()).format(formatDay),
        //    MDay    : moment(n.file.mday.toString()).format(formatDay),
        //    
        //},
    // File
        //{
        //    Path  : `[[${n.file.path}|${n.file.path}]]`,
        //    Size  : n.file.size,
        //    Ext   : n.file.ext,
        //},
    // file.tasks
        //dv.array(n.file.tasks).text,
        //dv.array(n.file.tasks).complete,
        //dv.array(n.file.tasks).fullyComplete,
        //dv.array(n.file.tasks).real,
        //dv.array(n.file.tasks).path,
        //dv.array(n.file.tasks).line,
        //dv.array(n.file.tasks).subtasks,
    // Inlinks
        //n.file.inlinks,
    // Outlinks
        //n.file.outlinks,
    // Requires
        //n?.requires?.split(',').map(v => {
        //    let r = this.app.plugins.getPlugin(v.trim())?.manifest.name;
        //    return `[[software~Obsidian~plugin~${v.trim()}|${r === undefined ? v.trim() : r}]]`;
        //}),
    // Source
        //{
        //    Target  : n?.source?.target,
        //    Template: n?.source?.template,
        //    Version : n?.source?.version,
        //},
    // Status
        //{
        //    Archived: n?.metadata?.status.archived ? (n?.metadata?.status.archived ? '✔' : '❌') : null,
        //    Reviewed: n?.metadata?.status.reviewed ? (n?.metadata?.status.reviewed ? '✔' : '❌') : null,
        //},
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
publish     :: true
requires    :: dataview, obsidian-icons-plugin, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/table, #Obsidian/plugin/dataview/js, #Obsidian/plugin/obsidian-icons-plugin, #Obsidian/plugin/templater-obsidian, javascript
title       :: 📦 table~dataview~js ⛓
type        :: template~block~table~plugin~dataview~js
uuid        :: 85a26ae5-9e0a-4c49-ac63-51a06bbc24b2
version     :: 1
```

## meta~todo

## meta~notes

## meta~inbox

*/
_%>