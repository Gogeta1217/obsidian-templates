<%* const tableLimit = 50; _%>
## `fas:Map` Table^[<% tableLimit %> of the most recently modified notes]

```dataviewjs

// performance timer begin

    const timerLabel = `${dv.current().file.path}:dataviewjs.table@${moment().format('YYYYMMDDHHmmss')}`;
        console.time(timerLabel);

// functions

    function getFrontmatter(p) {
        return app.metadataCache.metadataCache[`${app.metadataCache.getFileInfo(p)?.hash}`]?.frontmatter;
    }

    function getDataviewFields(p) {
        return app.plugins.getPlugin('dataview')?.index?.pages?.get(p)?.fields;
    }
    
    String.prototype.asRegExp = function() {
        var m = /^\/(?<regexp>.*)\/(?<flags>[a-z]*)$/.exec(this)?.groups;
        return new RegExp(m.regexp, m.flags);
    }

// variables

    const folderTemplates   = (this.app.plugins.getPlugin('templater-obsidian').settings.template_folder ?? this.app.internalPlugins.plugins.templates.instance.options.folder) ?? '';
    const library           = getFrontmatter(`${folderTemplates + '/'}📑 library~templates.md`);
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
            .where(n => n?.file?.folder.match('^' + dv.current().file.folder))
            //.where(n => n?.file?.name !== dv.current().file.name)
          
    // sort
    
            .sort(n => n?.file?.mtime.toString(), 'desc')
            
    // columns
    
            .map(n => {

            // functions

                String.prototype.tip = function(s) {
                    return `<span role='img' aria-label='${s}'>${this}</span>`
                }

                String.prototype.noWrap = function() {
                    return `<span style="white-space: nowrap">${this}</span>`
                }

            // variables

                const dvFields = getDataviewFields(n.file.path);
                const dvAliases = dvFields?.get('aliases');

                n.folderNote = `${n?.file?.folder}${'/' + folderNoteName}.md`;
                n.frontmatter = getFrontmatter(n.file.path);
                n.fullPath = app.vault.adapter?.getFullPath(n?.file?.folder.replace(/\\/g, '/')) ?? n?.file?.folder;
                
                if (n?.metadata === undefined) { n.metadata = {}; }
                n.metadata.type = n?.metadata?.type ? n?.metadata?.type : (
                    n?.type ? n?.type : (n?.file?.name.match(/~/) ? 
                    n?.file?.name.replace(matchEmoji, '').trim().split('~')[0] : 
                    n?.file?.name.replace(matchEmoji, '').trim().replace(/new/ig, ''))
                ) ?? null;
                
                n.parent        = n?.file?.folder.split('/').reverse();
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
                n.publishTip ? (n.publish || n.source.publish ? '✔' : '❌').toString().tip(n.publishTip) : null,

            // Created
                moment((n?.metadata?.datetime?.created?.toString() ?? n?.created?.toString()) ?? n?.file?.ctime.toString()).format(formatColumn).noWrap(),

            // Modified
                moment(n?.file?.mtime.toString()).format(formatColumn).noWrap(),

            // Scope
                n?.metadata?.scope ?? n?.scope,

            // Parent
                (app.vault.getAbstractFileByPath(n.folderNote) ? `[${n.parentString}](${encodeURI(n.folderNote)})`.tip(`open ${folderNoteName} of ${n.file.folder}`) : 
                    (`[${n.parentString}](` + encodeURI(`file:///${n.fullPath}`) + ')')
                    .tip(`open ${n.file.folder} in system explorer`)).noWrap(),

            // Title
                (n?.title ? `[[${n?.file?.path}|${n?.title}]]` : (n?.metadata?.title ? `[[${n?.file?.path}|${n?.metadata?.title}]]` : n?.file?.link)).toString(),

            // Aliases
                //Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',').map(v => v.trim()) : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]).concat(dvAliases?.length > 1 && typeof dvAliases === 'string' && dvAliases.match(/,/) ? dvAliases.split(',').map(v => v.trim()) : dvAliases).filter(v => v != null).map(v => `[[${n.file.path}|${v}]]`))).sort(),

            // Names
                //Array.from(new Set((n.file.aliases?.concat(typeof n?.aliases === 'string' ? n?.aliases.split(',') : null)).concat([n?.title, n?.metadata?.title, /* n.file.path,*/ n.file.name]).concat(dvAliases?.length > 1 && typeof dvAliases === 'object' && dvAliases[1].match(/,/) ? dvAliases[1].split(',') : null).filter(v => v != null).map(v => v.trim()).map(v => `[[${n.file.path}|${v}]]`))).sort(),

            // Description
                n?.metadata?.description ?? n?.description,

            // Type
                n?.metadata?.type === n?.file?.name ? null : n?.metadata?.type,

            // Tags
                //Array.from(new Set(n?.file?.tags.concat(typeof n?.tags === 'string' ? n?.tags.split(',').map(v => v.trim()) : null))).filter(v => v != null).sort().join(' '),

            // UUID
                //n?.uuid ? `\`${n?.uuid}\`` : null,

            // Version
                //n?.version ?? n?.source?.version,

            // Links   
                (n?.file?.outlinks?.length === 0 && n?.file?.inlinks?.length === 0) ? '❌'.tip('orphan') : `${ n?.file?.inlinks.length > 0 ? '🔽'.tip('backlinks') + '' + n?.file?.inlinks.length : ''}${ n?.file?.outlinks.length > 0 ? '🔼'.tip('outlinks') + '' + n?.file?.outlinks.length : ''}`.trim().noWrap(),

            // Task
                Array.from(new Set(dv.array(n?.file?.tasks).map(v => {
                    let m = v.text.match(/(?<t>\.*)/i)?.groups?.t.toLowerCase(); 
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
                //    CDay    : moment(n?.file?.cday.toString()).format(formatDay),
                //    MDay    : moment(n?.file?.mday.toString()).format(formatDay),
                //    
                //},

            // File
                //{
                //    Path  : `[[${n?.file?.path}|${n?.file?.path}]]`,
                //    Size  : n?.file?.size,
                //    Ext   : n?.file?.ext,
                //},

            // file.tasks
                //dv.array(n?.file?.tasks).text,
                //dv.array(n?.file?.tasks).complete,
                //dv.array(n?.file?.tasks).fullyComplete,
                //dv.array(n?.file?.tasks).real,
                //dv.array(n?.file?.tasks).path,
                //dv.array(n?.file?.tasks).line,
                //dv.array(n?.file?.tasks).subtasks,

            // Inlinks
                //n?.file?.inlinks,

            // Outlinks
                //n?.file?.outlinks,

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
                //    Archived: typeof n?.metadata?.status.archived === 'boolean' ? (n?.metadata?.status.archived ? '✔' : '❌') : null,
                //    Reviewed: typeof n?.metadata?.status.reviewed === 'boolean' ? (n?.metadata?.status.reviewed ? '✔' : '❌') : null,
                //},

                ]
            }
        )   
    // limit
    
        .slice(0, parseInt('<% tableLimit %>') > 0 ? parseInt('<% tableLimit %>') :  50)
        
    );
// performance timer end

    console.timeEnd(timerLabel);

```
<%-*
/*

# template

## meta~data

```dataviewfield
aliases     :: dataviewjs table
created     :: 2021-08-05T06:06:39-04:00
description :: `dataviewjs` table with enhanced template fields
publish     :: true
requires    :: dataview, folder-note-plugin, obsidian-icons-plugin, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/table, #Obsidian/plugin/dataview/js, #Obsidian/plugin/folder-note-plugin, #Obsidian/plugin/obsidian-icons-plugin, #Obsidian/plugin/templater-obsidian, javascript
title       :: 📦 table~dataview~js ⛓
type        :: template~block~table~plugin~dataview~js
uuid        :: 85a26ae5-9e0a-4c49-ac63-51a06bbc24b2
version     :: 1.1
```

## meta~todo

- [ ] #Obsidian/template/block/plugin/dataview/js/table/column/links/tip/pad add padding based on max string length #TODO
- [ ] #Obsidian/template/block/plugin/dataview/js/table/column/links/tip append concatenated list of outlinks/inlinks #TODO
    - may not be feasible with a tooltip
- [ ] #Obsidian/template/block/plugin/dataview/js/table/column/parent/path/parts split path and then apply span tag to parts before re-joining path #TODO
- [X] #Obsidian/template/block/plugin/dataview/js/table/column/parent/path display parent folder of parent folder (prefix with `.../`) when not located in `/` using `this.app.vault.getMarkdownFiles().filter(n => n.path.match(parentFolder))` or `this.app.metadataCache.uniqueFileLookup.get(fileName)` #TODO
    - allows path parts to roll over to new lines without divorcing them of their emoji counterparts
- [ ] #Obsidian/template/block/plugin/dataview/js/table/column/publish display corret emoji for publish state #TODO
- [ ] #Obsidian/template/block/plugin/dataview/js/table/column/tasks/tip display number of unfinished/completed tasks #TODO
- [ ] #Obsidian/template/block/plugin/dataview/js/table/refactor instead of adding properties to the map instance `n` declare constants and avoid mutating objects #TODO
- [ ] #Obsidian/template/block/plugin/dataview/js/table/refactor move field logic outside of return `[]` within `map()` and reference column variables instead #TODO
- [ ] #Obsidian/template/block/plugin/dataview/js/table/refactor simplify complex operations with better functions #TODO

## meta~notes

## meta~inbox

*/
_%>