---
# built-in [[obsidian~yaml]]
aliases : [📁 templates 🗺 overview, template overview, overview templates]
cssclass: 
publish : 
tags    : [Obsidian/plugins/folder-note, overview]
# for [[obsidian~plugin~breadcrumbs]]
container   : 
near        : 
contains    : 
# for [[obsidian~plugin~obsidian-advanced-uri]]
uuid        : 34b41476-5a80-4989-b8d3-d8a5f209d119
# this note
metadata:
    datetime: 
        created : 2021-08-05T14:56:11-04:00
        # from [[obsidian~plugin~obsidian-juliandate]]
        julian  : 2459432.12235
    description : files and file properties within templates directory
    scope       : 
    status      :
        archived: false
        reviewed: false
    title       : 🗺 Overview of 📁 templates
    type        : overview
# this note's template
source:
  # target  : "[[📁 templates/🗺 index.md]]"
    template: "[[📁 templates/📦 block~yaml.md]]"
    version : 1

---

# `=this.metadata.title`

**`=this.metadata.description`**, modified: _`$=moment(dv.current().file.mtime.toString()).format("LLLL")`_

---

## `fas:Map` Overview

```dataviewjs
dv.table(
    [// headings
        'Modified',
        'Scope',
        //'Title',
        //'Aliases',
        'Titles',
        'Description',
        'Type',
        //'Tags',
        'TODO',
        'Requires',
        'UUID',
        'Version',
        //'CSS Class',
        //'Publish',
        //'Breadcrumbs',
        //'Created',
        //'Datetime',
        //'Status',
        //'Source',
        //'File',
        //'Outlinks',
        //'Inlinks',
        //'Tasks',
        //'file.tasks.text',
        //'file.tasks.completed',
        //'file.tasks.fullyCompleted',
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
    // Modified
        moment(n.file.mtime.toString()).format('YYYYMMDD[T]HH:mm'),
    // Scope
        
        n?.metadata?.scope ?? n?.scope,
    // Title
        //n?.title ? `[[${n.file.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n.file.path}|${n?.metadata?.title}]]` : n.file.link),
    // Aliases
        //Array.from(new Set(n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Titles
        Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
    // Description
        n?.metadata?.description ?? n?.description,
    // Type
        n?.metadata?.type ? n?.metadata?.type : (
            n?.type ? n?.type : (n.file.name.match(/~/) ? 
            n.file.name.replace(/[^\x00-\x7F]+/g, '').trim().replace(/^template~/ig, '').split('~')[0] : 
            n.file.name.replace(/[^\x00-\x7F]+/g, '').trim().replace(/new/ig, ''))
        ).replace(/^template~/ig, ''),
    // Tags
        //Array.from(new Set(n.file.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),
    // TODO
        Array.from(new Set(dv.array(n.file.tasks).where(t => !t.completed).text.map(v => { return v.match(/(?<todo>\#todo)/i)?.groups?.todo.toLowerCase() }))).filter(v => v != null).sort().contains('#todo') ? '✔' : '❌',
    // Requires
        n?.requires?.split(',').map(v => {
            let r = this.app.plugins.getPlugin(v.trim())?.manifest.name;
            return `[[software~Obsidian~plugin~${v.trim()}|${r === undefined ? v.trim() : r}]]`;
        }),
    // UUID
        n?.uuid ? `\`${n?.uuid}\`` : '\\-',
    // Version
        n?.version ?? n?.source?.version,
    // CSS Class
        //n?.cssclass,
    // Publish
        //n?.publish,
    // Breadcrumbs
        //{
        //    Container   : n?.container,
        //    Near        : n?.alongside,
        //    Contains    : n?.contains,
        //},
    // Created
        //moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n.file.ctime.toString()).format('YYYYMMDD[T]HH:mm'),
    // Datetime
        //{
        //    Julian  : n?.metadata?.datetime.julian,
        //    CDay    : moment(n.file.cday.toString()).format('YYYY-MM-DD'),
        //    MDay    : moment(n.file.mday.toString()).format('YYYY-MM-DD'),
        //    
        //},
    // Status
        //{
        //    Archived: n?.metadata?.status.archived ? (n?.metadata?.status.archived ? "✔" : "❌") : "✖",
        //    Reviewed: n?.metadata?.status.reviewed ? (n?.metadata?.status.reviewed ? "✔" : "❌") : "✖",
        //},
    // Source
        //{
        //    Target  : n?.source?.target,
        //    Template: n?.source?.template,
        //    Version : n?.source?.version,
        //},
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
        //dv.array(n.file.tasks).where(t => { return !t.completed }),
        //dv.array(n.file.tasks).text,
        //dv.array(n.file.tasks).completed ? "✔" : "❌",
        //dv.array(n.file.tasks).fullyCompleted ? "✔" : "❌",
        //dv.array(n.file.tasks).real ? "✔" : "❌",
        //dv.array(n.file.tasks).path,
        //dv.array(n.file.tasks).line,
        //dv.array(n.file.tasks).subtasks,
    ]})
    // limit
    .slice(0, 50)
);
```

## Mentions 📁 templates

```query
"📁 templates" -path:(📁 templates) -path:(📁 templates)
```
