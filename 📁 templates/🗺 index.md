---
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid 34b41476-5a80-4989-b8d3-d8a5f209d119
# built-in [[obsidian~yaml]]
aliases : [*uuid, 📁 templates 🗺 overview, template table, overview of 📁 templates, 📁 templates]
cssclass: 
publish : 
tags    : [Obsidian/plugins/folder-note, overview, table]
# for [[software~Obsidian~plugin~breadcrumbs]]
container   : 
near        : 
contains    : 
# this note
metadata:
    datetime: 
        created : 2021-08-05T14:56:11-04:00
        # from [[software~Obsidian~plugin~obsidian-juliandate]]
        julian  : 2459432.12235
    description : "files and file properties within [`📁 templates`](file:///C:%5CUsers%5CUser%5CDocuments%5CObsidian%5C%F0%9F%8F%A0%20Personal%20%F0%9F%97%84%5C%F0%9F%93%81%20templates)"
    scope       : 
    status      :
        archived: false
        reviewed: false
    title       : 🗺 Overview of 📁 templates
    type        : note~overview~table
# this note's template
source:
    publish : true
  # target  : '[[📁 templates/🗺 index.md]]'
    template: '[[📁 templates/📦 block~yaml.md]]'
    version : 2

---

# `=this.metadata.title`

**`=this.metadata.description`**, modified: _`$=moment(dv.current().file.mtime.toString()).format('LLLL')`_

---

## `fas:Map` Table

```dataviewjs
const format = 'YYYYMMDD[T]HH:mm';
const formatDay = 'YYYY-MM-DD'
const matchEmoji = /[^\x00-\x7F]+/g;
const folderNoteName = this.app.plugins.getPlugin('folder-note-plugin')?.settings?.folderNoteName;
const breadcrumbs = this.app.plugins.getPlugin('breadcrumbs')?.settings;
dv.table(
    [// headings
        'Publish',
        //'Created',
        'Modified',
        'Scope',
        //'Parent',
        //'Title',
        //'Aliases',
        'Names',
        'Description',
        'Type',
        //'Tags',
        'Requires',
        //'UUID',
        'Version',
        'TODO',
        //'CSS Class',
        //'Breadcrumbs',
        //'Datetime',
        //'File',
        //'Inlinks',
        //'file.tasks.text',
        //'file.tasks.completed',
        //'file.tasks.fullyCompleted',
        //'file.tasks.real',
        //'file.tasks.path',
        //'file.tasks.line',
        //'file.tasks.subtasks',
        //'Outlinks',
        //'Source',
        //'Status',
    ],
    // filter
    dv.pages('')
    .where(n =>  n.file.folder === dv.current().file.folder)
    //.where(n =>  n.file.name !== dv.current().file.name)
    // sort
    .sort(n => n.file.mtime.toString(), 'desc')
    .sort(n => (Array.from(new Set(dv.array(n.file.tasks).map(v => {
            let m = v.text.match(/(?<todo>\#todo)/i)?.groups?.todo.toLowerCase(); 
            return m !== undefined ? `${m}/${v.completed}` : null
        }))).filter(v => v != null).sort().map(v => { return v.match('true') ? 'true' : 'false' }).sort(v => v, 'desc')[0] ?? 'null'))
    .sort(n => ((n?.publish ?? n?.source?.publish) ? ((n?.publish ?? n?.source?.publish) ? true : false) : null), 'desc')
    // columns
    .map(n => {
        return [
    // Publish
        (n?.publish ?? n?.source?.publish) ? ((n?.publish ?? n?.source?.publish) ? '✔' : '❌') : null,
    // Created
        //moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n.file.ctime.toString()).format(format),
    // Modified
        moment(n.file.mtime.toString()).format(format),
    // Scope
        n?.metadata?.scope ?? n?.scope,
    // Parent
        //`[${n.file.folder.replace(/.*\/([^\/]+)$/, '$1')}](` + (app.vault.getAbstractFileByPath(folderNote) ? encodeURI(folderNote) : encodeURI(`file:///${app.vault.adapter.getFullPath(n.file.folder.replace(/\\/g, '/'))}`)) + ')',
    // Title
        //n?.title ? `[[${n.file.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n.file.path}|${n?.metadata?.title}]]` : n.file.link),
    // Aliases
        //Array.from(new Set(n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Names
        Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Description
        n?.metadata?.description ?? n?.description,
    // Type
        n?.metadata?.type ? n?.metadata?.type : (
            n?.type ? n?.type : (n.file.name.match(/~/) ? 
            n.file.name.replace(matchEmoji, '').trim().replace(/^template~/ig, '').split('~')[0] : 
            n.file.name.replace(matchEmoji, '').trim().replace(/new/ig, ''))
        ).replace(/^template~/ig, ''),
    // Tags
        //Array.from(new Set(n.file.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),
    // Requires
        n?.requires?.split(',').map(v => {
            let r = this.app.plugins.getPlugin(v.trim())?.manifest.name;
            return `[[software~Obsidian~plugin~${v.trim()}|${r === undefined ? v.trim() : r}]]`;
        }),
    // UUID
        //n?.uuid ? `\`${n?.uuid}\`` : null,
    // Version
        n?.version ?? n?.source?.version,
    // TODO
        Array.from(new Set(dv.array(n.file.tasks).map(v => {
            let m = v.text.match(/(?<todo>\#todo)/i)?.groups?.todo.toLowerCase(); 
            return m !== undefined ? `${m}/${v.completed}` : null
        }))).filter(v => v != null).sort().map(v => { return v.match('true') ? '✔' : '❌' }).sort(v => v, 'desc')[0] ?? null,
    // CSS Class
        //n?.cssclass,
    // Breadcrumbs
        //{
        //    [breadcrumbs.parentFieldName]   : n[breadcrumbs.parentFieldName],
        //    [breadcrumbs.siblingFieldName]  : n[breadcrumbs.siblingFieldName],
        //    [breadcrumbs.childFieldName]    : n[breadcrumbs.breadcrumbs],
        //},
    // Datetime
        //{
        //    Julian  : n?.metadata?.datetime.julian,
        //    CDay    : moment(n.file.cday.toString()).format(formatDay),
        //    MDay    : moment(n.file.mday.toString()).format(formatDay),
        //    
        //},
    // File
        //{
        //    Path    : `[[${n.file.path}|${n.file.path}]]`,
        //    Size    : n.file.size,
        //    Ext     : n.file.ext,
        //},
    // file.tasks
        //dv.array(n.file.tasks).text,
        //dv.array(n.file.tasks).completed ? '✔' : '❌',
        //dv.array(n.file.tasks).fullyCompleted ? '✔' : '❌',
        //dv.array(n.file.tasks).real ? '✔' : '❌',
        //dv.array(n.file.tasks).path,
        //dv.array(n.file.tasks).line,
        //dv.array(n.file.tasks).subtasks,
    // Inlinks
        //n.file.inlinks,
    // Outlinks
        //n.file.outlinks,
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
    //.slice(0, 50)
);
```

## `fas:Comment` Mentions 📁 templates

```query
"📁 templates" -path:(📁 templates)
```

## Inbox
