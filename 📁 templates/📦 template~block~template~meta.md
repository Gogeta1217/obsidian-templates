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
aliases     :: 
description :: template for x entries
requires    :: plugin~y
scope       :: 
tags        :: [Obsidian/template/type/x]
title       :: 
type        :: template~type~x
```

## meta~todo

- [ ] manually run template through #Obsidian/plugin/Templater replacing `/` to render 'stage 2' which should prompt for input with current value like in [[📄 new uri 🔗]] - rough example: `description :: </%* tR += ?? {dataviewcurrent}.description "template for x entries" %>`
    - allows us to preserve existing dataview inline fields but replace everything else

## meta~notes

- include statement has to strip code and metadata like `<%* tR += (await tp.file.include('[[📦 template~block~template~meta]]')).replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, ''); %>`

## meta~inbox

*/
_%>
<%* 
if (tp.config.run_mode === 1) {
    tR = tp.config.template_file.unsafeCachedData.replace(/^\<\%[-_+]?\*[-_+]?.*\%\>/gms, '').replace(/<\\%/g, '<%').replace(/:\\:/g, '::').replace(/\\#/g, '#').replace(/\n+$/, '');
}
%>