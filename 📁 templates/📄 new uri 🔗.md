---
<%_* 
const global_regex      = Object.fromEntries(Object.entries(await this.app.plugins.getPlugin('dataview').api.page('📁 data/global.md').regex).map(entry => { 
                            regex = entry[1].match(/(\/?)(.+)\1([a-z]*)/i); 
                            return [entry[0], new RegExp(regex[2], regex[3])];
                        } ));                                                               // regex library
let parsedResource;                                                                         // used to split the uri into parts
let parsedFileName      = tp.file.title.match(global_regex.matchPathCustom)?.groups         // split the filename into parts
let uri                 = {};                                                               // uri data
async function promptUri() {
    return await tp.system.prompt('Enter URI', 'scheme://www.example.null', true);
}
console.log(tp.config.run_mode);
if (tp.config.run_mode  === 1) {                                                            // new note from templater-obsidian:insert-templater
    uri.data            = (tp.frontmatter?.uri?.data ?? await promptUri());                // prompt for uri
}
if (tp.config.run_mode  === 2) {                                                            // new note from link click
    uri.data            = parsedFileName?.resource.match(global_regex.matchUrlAny) ? 
        parsedFileName?.resource : await promptUri();                                       // get uri from title else prompt
}
uri.data                = encodeURI(uri.data.match(global_regex.matchUrlSchemeAny) ? 
                            uri.data : `https://${uri.data}`);                              // check uri from scheme and add default https scheme if false
console.log("here again");
parsedResource          = uri.data.match(global_regex.matchUrlAny).groups;                  // re-parse uri
if (parsedResource.subdomain.split('.')[0] === 'www' && parsedResource.subdomain.split('.')[1] === parsedResource.domain) {             // uri contains www
    uri.data = `${parsedResource.scheme?? ''}${parsedResource.domain ?? ''}.${parsedResource.tld ?? ''}${parsedResource.path ?? ''}`;   // recreate uri w/o www
    parsedResource     = uri.data.match(global_regex.matchUrlAny).groups;                                                               // re-parse uri
}
uri.label              = tp.frontmatter?.uri?.label ?? (uri.data.match(global_regex.matchUrlSchemeHttp) ? 
                            await this.app.plugins?.getPlugin('obsidian-auto-link-title')?.fetchUrlTitle(uri.data) : 
                                `common name for ${uri.data}`)                                                              // if http(s) scheme try to fetch uri title
uri.label              = await tp.system.prompt(`Label for ${uri.data}`, uri.label, true);                                  // prompt for label
uri.path               = tp.frontmatter?.uri?.path ?? await tp.system.prompt(`Storage path for ${uri.data}`, '', true);     // prompt for path
uri.type               = await tp.system.suggester(['Bookmark entry', 'Allow list entry', 'Deny list entry'], 
                            ['bookmark', 'allowlist', 'denylist'], true, `URI type for ${uri.data}`);                       // prompt for type
uri.description        = await tp.system.prompt(`Use case for ${uri.data}`, (tp.frontmatter?.metadata?.description ? 
                            tp.frontmatter?.metadata?.description?.replace(/\\/g, '\\') : 
                                `URI page for [${uri.label}](${uri.data}) which is a ${uri.type} entry`), true);            // prompt for description
%>
uri     : 
    data    : <%* tR += `&uri.data ${uri.data}`; %>
    label   : <%* tR += uri.label; %>
    path    : <%* tR += uri.path; %>
    type    : <%* tR += uri.type;  %>
<%*
tR += (await tp.file.include('[[📦 block~yaml]]'))
            // remove yaml directives
        .replace(/^---/gm, '')
            // replace aliases
        .replace(/^((?:[ ]+)?aliases(?:[ ]+)?:)(?:[ ]+)?$/gm,       `$1 [*uri.data, "${uri.type}~${uri.label}"]`)
            // replace tags
        .replace(/^((?:[ ]+)?tags(?:[ ]+)?:)(?:[ ]+)?$/gm,          `$1 [${(await tp.system.prompt('Tags', tp.frontmatter?.tags?.join(', ') ?? 
            'Obsidian/uri', true))?.replace(/\s+/g,'').trim().split(',').join(', ')}]`)
            // preserve existing uuid
        .replace(/^((?:[ ]+)?uuid(?:[ ]+)?:)(?:[ ]+)?(.*)$/gm,       `$1 ${tp.frontmatter?.uuid ?? '$2'}`)
            // replace metadata.description
        .replace(/^((?:[ ]+)?description(?:[ ]+)?:)(?:[ ]+)?$/gm,   `$1 "${uri.description}"`)
            // replace metadata.title
        .replace(/^((?:[ ]+)?title(?:[ ]+)?:)(?:[ ]+)?.*$/gm,       `$1 uri~${uri.data}`)
            // replace metadata.type
        .replace(/^((?:[ ]+)?type(?:[ ]+)?:)(?:[ ]+)?$/gm,          `$1 ${tp.config.template_file.basename
                                                                        .replace(/[^\x00-\x7F]+/g,'')
                                                                        .replace(/new/, '')
                                                                        .toLowerCase().trim()}`)
        .trim();
%>

---

<%* tR += (await tp.file.include('[[📦 block~header]]')); %>

---

## Links <%* tR += uri.data; %>

```query
/<%*
    // remove illegal characters from uri for rename
const uniquefileName    = `uri~${uri.data
                            .replace(/^[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+/g, '')
                            .replace(/[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+$/g, '')
                            .replace(/[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+/g, '_')}`;
    // rename file
await tp.file.rename(uniquefileName);
    // output #Obsidian/query/regex for uri
tR                      += (parsedResource.subdomain === `${parsedResource.domain ?? ''}.${parsedResource.tld ?? ''}` ? 
                            `${parsedResource.scheme ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&') + `(www\\.)?` + `${parsedResource.subdomain ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&') + `${parsedResource.path ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&') : uri.data.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&')) + '(?:\\/.*)?'; 
%>/ -file:(<%* tR += uniquefileName; %>)
```

## Inbox

<%-*
/*
# meta

## yaml

```
aliases     :: uri note template
description :: template for uri entries
requires    :: [obsidian-auto-link-title, obsidian-dataview, Templater]
scope       :: 
tags        :: [Obsidian/template/note/uri, Obsidian/plugin/obsidian-auto-link-title, Obsidian/plugin/obsidian-dataview, Obsidian/plugin/Templater]
title       :: uri note template
type        :: template~note~uri
```

## todo

- [ ] #Obsidian/template/uri/regex match metadata.<subproperly> properly to avoid mismatching
- [ ] #Obsidian/template/uri/rename check for existing file, cancel generation, delete file and then open existing file?

## notes

## inbox

*/
_%>