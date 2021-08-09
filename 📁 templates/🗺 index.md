---
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid 34b41476-5a80-4989-b8d3-d8a5f209d119
# built-in [[obsidian~yaml]]
aliases : [*uuid, "templates 📁 overview 🗺", "overview 🗺 of templates 📁", "templates 📁 table ⛓", "table ⛓ of templates 📁", "📁 templates"]
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
    description : "files and file properties within [`📁 templates`](file:///<uri/encoded/path/to/templates/folder>/5C%F0%9F%93%81%20templates)"
    scope       : 
    status      :
        archived: false
        reviewed: false
    title       : 🗺 overview of 📁 templates
    type        : overview~table
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

## `fas:Map` Table^[50 of the most recently modified templates]


```dataviewjs

// performance timer begin

    const timerLabel = `${dv.current().file.path}:dataviewjs.table@${moment().format('YYYYMMDDHHmmss')}`;
        console.time(timerLabel);

// table functions

    function getFrontmatter(p) {
        return app.metadataCache.metadataCache[`${app.metadataCache.getFileInfo(p)?.hash}`]?.frontmatter;
    }

    function getDataviewFields(p) {
        return app.plugins.getPlugin('dataview')?.index?.pages?.get(p)?.fields;
    }
    
  // prototypes
    
    String.prototype.asRegExp = function() {
        var m = /^\/(?<regexp>.*)\/(?<flags>[a-z]*)$/.exec(this)?.groups;
        return new RegExp(m.regexp, m.flags);
    }
    
    String.prototype.tip = function(s) {
        return `<span role='img' aria-label='${s}'>${this}</span>`;
    }

    String.prototype.noWrap = function() {
        return `<span style="white-space: nowrap">${this}</span>`;
    }

// table variables

    const folderTemplates   = (this.app.plugins.getPlugin('templater-obsidian')?.settings?.template_folder ??this.app.internalPlugins.plugins.templates.instance.options.folder) ?? '';
    const library           = getFrontmatter(`${folderTemplates + '/'}📑 library~template.md`);
    const formatColumn      = library?.settings?.datetime?.formatColumn ?? 'YYYY-MM-DD HH:mm';
    const formatDay         = library?.settings?.datetime?.formatDay ?? 'YYYY-MM-DD'
    const matchEmoji        = library?.regex?.matchEmoji?.asRegExp();
    const folderNoteName    = this.app.plugins.getPlugin('folder-note-plugin')?.settings?.folderNoteName;
    const breadcrumbs       = this.app.plugins.getPlugin('breadcrumbs')?.settings;
    
// table

    dv.table(
        [
        
    // headings
    
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
        'Mobile',
        'Requires',
        //'UUID',
        'Version',
        'Links',
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
            .where(n => n.file.folder === dv.current().file.folder)
            //.where(n => n.file.name !== dv.current().file.name)

    // sort
    
        .sort(n => n.file.mtime.toString(), 'desc')
        .sort(n => (Array.from(new Set(dv.array(n.file.tasks).map(v => {
                let m = v.text.match(/(?<todo>\#todo)/i)?.groups?.todo.toLowerCase(); 
                return m !== undefined ? `${m}/${v.completed}` : null
            }))).filter(v => v != null).sort().map(v => { return v.match('true') ? 'true' : 'false' }).sort(v => v, 'desc')[0] ?? 'null'))
        .sort(n => ((n?.publish ?? n?.source?.publish) ? ((n?.publish ?? n?.source?.publish) ? true : false) : null), 'desc')
    
    // columns
    
        .map(n => {

            // map variables
                
                const dvFields = getDataviewFields(n.file.path);
                const dvAliases = dvFields?.get('aliases');
                
                n.folderNote = `${n?.file?.folder}${'/' + folderNoteName}.md`;
                n.frontmatter = getFrontmatter(n.file.path);
                //n.fullPath = app.vault.adapter?.getFullPath(n?.file?.folder.replace(/\\/g, '/')) ?? n?.file?.folder;
                
                if (n?.metadata === undefined) { n.metadata = {}; }
                n.metadata.type = n?.metadata?.type ? n?.metadata?.type : (
                    n?.type ? n?.type : (n?.file?.name.match(/~/) ? 
                    n?.file?.name.replace(matchEmoji, '').trim().split('~')[0] : 
                    n?.file?.name.replace(matchEmoji, '').trim().replace(/new/ig, ''))
                ).toString().trim() ?? null;
                
                n.parent = n?.file?.folder.split('/').reverse();
                    switch (true) {
                        case (n.parent.length > 2):
                            n.parentString = `../${n.parent[1]}/${n.parent[0]}`;
                            break;
                        case (n.parent.length > 1):
                            n.parentString = `${n.parent[1]}/${n.parent[0]}`;
                            break;
                        case (!n.file.folder):
                            n.parentString = this.app.vault.adapter.getName() ?? '/';
                            break;
                        default:
                            n.parentString = n.parent[0];
                            break;
                    }
                    
                // have to manually inspect dataview fields because it combines yaml and inline but prefers the yaml value when both are set
                n.publishTip = Object.entries({
                    ['dataview.publish']    : (typeof dvFields?.get('publish') === 'boolean' && typeof n?.frontmatter?.publish !== 'boolean') || dvFields?.get('publish')?.length > 1,
                    ['yaml.publish']        : (typeof n?.frontmatter?.publish === 'boolean' && typeof n?.publish !== 'boolean') || (typeof n?.frontmatter?.publish === 'boolean' && typeof n?.publish === 'boolean'),
                    ['yaml.source.publish'] : typeof n?.source?.publish === 'boolean', 
                }).filter(v => v[1] === true).map(v => v[0]).sort().join(", ");
                
                // #TODO add pading to links per max digit length
                //const linkPadding = '';
        
        // bindings
        
                return [

            // Publish
                    // #TODO find a better way to do this and return '✔', '❌', or '❔' depending on the values
                n.publishTip !== undefined ? (n?.publish || n?.source?.publish ? '✔' : '❌').toString().tip(n.publishTip) : null,
                
            // Created
                //moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n?.file?.ctime.toString()).format(formatColumn).noWrap(),
                
            // Modified
                moment(n?.file?.mtime.toString()).format(formatColumn).noWrap(),
                
            // Scope
                n?.metadata?.scope ?? n?.scope,
                
            // Parent
                //(app.vault.getAbstractFileByPath(n.folderNote) ? `[${n.parentString}](${encodeURI(n.folderNote)})`.tip(`open ${folderNoteName} of ${n.file.folder}`) : 
                //    (`[${n.parentString}](` + encodeURI(`file:///${n.file.path}`) + ')')
                //    .tip(`open ${n.file.folder} in system explorer`)).noWrap(),
                
            // Title
                //n?.title ? `[[${n.file.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n.file.path}|${n?.metadata?.title}]]` : n.file.link),
                
            // Aliases
                //Array.from(new Set(n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null))).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`).sort(),
                
            // Names
                Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',') : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]).concat(dvAliases?.length > 1 && typeof dvAliases === 'object' && dvAliases[1].match(/,/) ? dvAliases[1].split(',') : null).filter(v => v != null).map(v => v.trim()).map(v => `[[${n.file.path}|${v}]]`))).sort(),
                
            // Description
                n?.metadata?.description ?? n?.description,
                
            // Type
                n?.metadata?.type === n?.file?.name ? null : n.metadata.type.replace(new RegExp(`^${n.file.folder.replace(matchEmoji, '').trim()}?~`.trim(), 'g'), ''),
                
            // Tags
                //Array.from(new Set(n.file.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),
                
            // Mobile
                typeof n?.mobile === 'boolean' ? (n?.mobile ? '✔'.tip('supported') : '❌'.tip('unsupported')) : '❔',
                
            // Requires
                n?.requires?.split(',').map(v => {
                    let r = this.app.plugins.getPlugin(v.trim())?.manifest.name;
                    return `[[software~Obsidian~plugin~${v.trim()}|${r === undefined ? v.trim() : r}]]`;
                }),
                
            // UUID
                //n?.uuid ? `\`${n?.uuid}\`` : null,
                
            // Version
                n?.version ?? n?.source?.version,
                
            // Links   
                (n?.file?.outlinks?.length === 0 && n?.file?.inlinks?.length === 0) ? '❌'.tip('orphan') : `${ n?.file?.inlinks.length > 0 ? '🔽'.tip('backlinks') + '' + n?.file?.inlinks.length : ''}${ n?.file?.outlinks.length > 0 ? '🔼'.tip('outlinks') + '' + n?.file?.outlinks.length : ''}`.trim().noWrap(),
                
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
                //    Archived: typeof n?.metadata?.status.archived === 'boolean' ? (n?.metadata?.status.archived ? '✔' : '❌') : null,
                //    Reviewed: typeof n?.metadata?.status.reviewed === 'boolean' ? (n?.metadata?.status.reviewed ? '✔' : '❌') : null,
                //},
                
                ]
            }
        )
    // limit
    
        //.slice(0, 50)
        
    );

// performance timer end

    console.timeEnd(timerLabel);

```

## TODO

- [ ] #Obsidian/template/-/structure/refactor add better comments and more functions for redundant code #TODO
- [ ] #Obsidian/template/-/reference/refactor use '\[[\]]' syntax when referencing files within code (ex: [[📑 library~template]]) so that rename events update references #TODO
    - does this work in codeblocks? #INVESTIGATE
- [ ] #Obsidian/template/-/component/yaml/refactor upgrade yaml logic #TODO

## `fas:Comment` Mentions 📁 templates

```query
"📁 templates" -path:(📁 templates)
```

## Inbox
