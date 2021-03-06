<\%-*
/*

\# template

\## meta~data

```dataviewfield
aliases     :\: x type
created     :\: <%* tR += moment().format(); %>
description :\: template for x entries
mobile      :\:
publish     :\: 
requires    :\: plugin~y
scope       :\: 
tags        :\: \#Obsidian/template/type/x
title       :\: <% tp.file.title %>
type        :\: template~type~x
uuid        :\: <%* tR += ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)); %>
version     :\: 0.1
```

\## meta~todo

\## meta~notes

\## meta~inbox

*/
_%>
<%-*
/*

# template

## meta~data

```dataviewfield
aliases     :: meta
created     :: 2021-07-28T01:37:29-04:00
description :: `meta` section (data + headings) for templates
mobile      :: false
publish     :: true
requires    :: dataview, templater-obsidian
scope       :: 
tags        :: #Obsidian/template/block/template/meta
title       :: 📦 meta ✉
type        :: template~block~meta
uuid        :: 36a93245-3eaf-41ef-a27d-b8e5110e508e
version     :: 0.1
```

## meta~todo

- [ ] #Obsidian/template/block/meta/output manually run template through #Obsidian/plugin/Templater ~~replacing `/`~~ to render 'stage 2' version which should prompt for input with current values like in [[📁 templates/📄 new uri 🔗]] - rough example: `description :\: <\%* let description = {dataviewcurrent}.description ?? ''; description = await tp.system.prompt('description', description); tR += description; %\>` #TODO
    - allows us to preserve existing dataview inline fields but replace everything else
    - i only need to do the manual templater step when `tp.config.run_mode === 1`
    - i may be able to avoid this by encapsulating literal output with '' between `<%` and `%>`
- [ ] #Obsidian/template/block/meta/version update version and formatting semi~automatically #TODO
    - ideally, we'll select the meta block and re-insert via Templater, preserving meta and heading data
    - i'm currently outputting a template file's `file.mtime` to the yaml field `source.version` but this isn't very reliable
- [ ] #Obsidian/template/block/meta/mobile fix on mobile #TODO

## meta~notes

- `tp.file.include` statement has to strip code and metadata like `<\%* tR += (await tp.file.include('[[📦 template~block~template~meta]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>`
- metadata is pretty redundant in this example but the extra fields may come in handy later
- since i broke `templater-obsidian:insert-templater` with this template, i have been opening the console and using `copy()` with unreplaced template code 

## meta~inbox

*/
_%>
<%* 
if (tp.config.run_mode === 1) {
    tR = tp.config.template_file.unsafeCachedData.replace(/^\<\%[-_+]?\*[-_+]?.*\%\>/gms, '').replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, '');
}
%>