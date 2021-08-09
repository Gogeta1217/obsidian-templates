---
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid 9098c2af-3ddf-4ad4-b488-2b634e455440
# built-in [[software~Obsidian#yaml]]
aliases : [*uuid, "obsidian plugins 🔌 overview 🗺", "overview 🗺 of obsidian plugins 🔌", "obsidian plugin 🔌 table ⛓", "table ⛓ of obsidian plugin 🔌"]
cssclass: 
publish : 
tags    : [Obsidian/plugin, overview, index, table]
# for [[software~Obsidian~plugin~breadcrumbs]]
conatiner   : 
near        : 
contains    : 
# this note
metadata:
    datetime: 
        created : 2021-08-06T12:31:34-04:00
        # from [[software~Obsidian~plugin~obsidian-juliandate]]
        julian  : 2459433.02193
    description : "plugin properties for [[software~Obsidian#plugins|Obsidian plugins]"
    scope       : 
    status      :
        archived: false
        reviewed: false
    title       : 🗺 Overview of Obsidian plugins 🔌
    type        : overview~table
# this note's template
source:
    publish : true
  # target  : "[[📂 nonhierarchical/🗺 table~software~Obsidian~plugin 🔌.md]]"
    template: "[[📁 templates/📦 template~plugin~folder-note~index 🗺.md]]"
    version : 2

---

# `=this.metadata.title`

**`=this.metadata.description`**, modified: _`$=moment(dv.current().file.mtime.toString()).format("LLLL")`_

---

## `fas:Map` Table

```dataviewjs
const timerLabel = `${dv.current().file.path}:update@${moment().format('YYYYMMDDHHmmss')}`;
console.time(timerLabel);
const basePath = this.app.vault.adapter.getBasePath();
let pluginManifest = Array.from(new Map(Object.entries(this.app.plugins.manifests)).entries());
//const {createButton} = app.plugins.getPlugin('buttons')
dv.table(
    [
        //'key',
        'ID',
        '🧩 Note',
        'Version',
        'Author',
        'Description',
        '📁',
        'Desktop Only',
        'App Version',
        'Loaded',
        //'Button',
        //'settings'
    ],
    dv.array(pluginManifest)
    .sort(p => p[1].name, 'asc')
    .sort(p => p[1].isDesktopOnly, 'desc')
    .sort(p => this.app.plugins.getPlugin(p[0])?._loaded ?? false, 'desc')
    .map(p => [
    //p[0],
    p[1].id,
    `[[software~Obsidian~plugin~${p[1].id}|${p[1].name}]]`,
    p[1].version,
    p[1].authorUrl ? (
            p[1].author ? `[${p[1].author}](${p[1].authorUrl})` : p[1].authorUrl
        ) : (
            p[1].author ? p[1].author : null
        ),
    p[1].description,
    '[🔗](' + encodeURI(`file:///${basePath}/${p[1].dir.replace(/\\/g, '/')}`) + ')',
    p[1].isDesktopOnly ? '✔' : '❌',
    p[1].minAppVersion,
    (this.app.plugins.getPlugin(p[0])?._loaded ?? false) ? '✔' : '❌',
    //createButton({app, el: this.container, args: {name: "🔌"}, clickOverride: {click: '', params: ['Status', 'Completed', p.file?.path]}})
    //this.app.plugins.getPlugin(p[0])?.settings
    ])
);
console.timeEnd(timerLabel);
```

## `fas:Comment` Mentions 🗺 table~software~Obsidian~plugin 🔌

```query
"🗺 table~software~Obsidian~plugin 🔌" -file:(🗺 table~software~Obsidian~plugin 🔌)
```

## Inbox