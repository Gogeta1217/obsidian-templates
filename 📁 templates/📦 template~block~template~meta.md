<\%-*
/*
\# template

\## meta~yaml

```dataviewfielda
aliases     :\: 
description :\: template for x entries
requires    :\: plugin~y
scope       :\: 
tags        :\: [Obsidian/template/type/x]
title       :\: 
type        :\: template~type~x
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

```dataviewfielda
aliases     :: template meta
description :: meta block with headings for templates
requires    :: dataview, Templater
scope       :: 
tags        :: [Obsidian/template/block/meta]
title       :: 📦 template~block~meta
type        :: template~block~meta
```

## meta~todo

- [ ] manually run template through #Obsidian/plugin/Templater replacing `/` to render 'stage 2' version which should prompt for input with current values like in [[📄 new uri 🔗]] - rough example: `description :\: </%* let description = {dataviewcurrent}.description ?? ''; description = await tp.system.prompt('description', description); tR += description; %>`
    - allows us to preserve existing dataview inline fields but replace everything else

## meta~notes

- `tp.file.include` statement has to strip code and metadata like `<%* tR += (await tp.file.include('[[📦 template~block~template~meta]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>`
- metadata is pretty redundant in this example but the extra fields may come in handy later

## meta~inbox

*/
_%>
<%* 
if (tp.config.run_mode === 1) {
    tR = tp.config.template_file.unsafeCachedData.replace(/^\<\%[-_+]?\*[-_+]?.*\%\>/gms, '').replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, '');
}
%>