---
<%_* 
async function promptUri() {
    return await tp.system.prompt('Enter URI', 'scheme://www.example.null', true);
}
const matchUrlAny = /^(?<scheme>(?:(?:[\w\d-+]+):)?(?:\/\/)?)(?:(?:(?<username>\S+)(?::(?<password>\S*)))?@)?(?<subdomain>(?!(?:10|127)(?:\.\d{1,3}){3})(?!(?:169\.254|192\.168)(?:\.\d{1,3}){2})(?!172\.(?:1[6-9]|2\d|3[0-1])(?:\.\d{1,3}){2})(?:[1-9]\d?|1\d\d|2[01]\d|22[0-3])(?:\.(?:1?\d{1,2}|2[0-4]\d|25[0-5])){2}(?:\.(?:[1-9]\d?|1\d\d|2[0-4]\d|25[0-4]))|(?:(?<domain>(?:[a-z0-9\u00a1-\uffff][a-z0-9\u00a1-\uffff_-]{0,62})?[a-z0-9\u00a1-\uffff])\.)+(?<tld>[a-z\u00a1-\uffff]{2,}\.?))(?::(?<port>\d{2,5}))?(?<path>[/?#]\S*)?$/i
const matchUrlSchemeAny = /^(?:(?:(?:[\w\d-+]+):)(?:\/\/)?)(?:\S+(?::\S*)?@)?(?:(?!(?:10|127)(?:\.\d{1,3}){3})(?!(?:169\.254|192\.168)(?:\.\d{1,3}){2})(?!172\.(?:1[6-9]|2\d|3[0-1])(?:\.\d{1,3}){2})(?:[1-9]\d?|1\d\d|2[01]\d|22[0-3])(?:\.(?:1?\d{1,2}|2[0-4]\d|25[0-5])){2}(?:\.(?:[1-9]\d?|1\d\d|2[0-4]\d|25[0-4]))|(?:(?:[a-z0-9\u00a1-\uffff][a-z0-9\u00a1-\uffff_-]{0,62})?[a-z0-9\u00a1-\uffff]\.)+(?:[a-z\u00a1-\uffff]{2,}\.?))(?::\d{2,5})?(?:[/?#]\S*)?$/i
const matchUrlSchemeHttp = /^(?:(?:(?:https?):)\/\/)/i
const matchFileNameCustom = /(?!(?:\s+)?(?<scope>.*)(?:\s+)?!(?:\s+)?)?(?:(?<type>.*?)~)?(?:(?<path>(?:(?:.*))+)~)?(?<resource>.*)?/i
// used to split the uri into parts
let parsedUri;
// split the filename into parts
let parsedFileName = tp.file.title.match(matchFileNameCustom)?.groups
// uri data
const uri = {};
// new note from templater-obsidian:insert-templater
if (tp.config.run_mode === 1) {
    // prompt for uri
    uri.data = (tp.frontmatter?.uri?.data ?? await promptUri());
}
// new note from link click
if (tp.config.run_mode === 2) {
    // get uri from title else prompt
    uri.data = parsedFileName?.resource.match(matchUrlAny) ? parsedFileName?.resource : await promptUri();
}
// check uri from scheme and add default https scheme if false
uri.data = encodeURI(uri.data.match(matchUrlSchemeAny) ? uri.data : `https://${uri.data}`);
// re-parse uri
parsedUri = uri.data.match(matchUrlAny).groups;
// uri contains www
if (parsedUri.subdomain.split('.')[0] === 'www' && parsedUri.subdomain.split('.')[1] === parsedUri.domain) {
    // recreate uri w/o www
    uri.data = `${parsedUri.scheme?? ''}${parsedUri.domain ?? ''}.${parsedUri.tld ?? ''}${parsedUri.path ?? ''}`;
    // re-parse uri
    parsedUri = uri.data.match(matchUrlAny).groups;
}
// if http(s) scheme fetch url title
uri.label = tp.frontmatter?.uri?.label ?? (uri.data.match(matchUrlSchemeHttp) ? 
    await this.app.plugins?.getPlugin('obsidian-auto-link-title')?.fetchUrlTitle(uri.data) : 
    `common name for ${uri.data}`)
// prompt for label
uri.label = await tp.system.prompt(`Label for ${uri.data}`, uri.label, true);
// prompt for path
uri.path = (tp.frontmatter?.uri?.path ?? parsedFileName?.type) ?? await tp.system.prompt(`Storage path for ${uri.data}`, '', true);
// prompt for type
uri.type = await tp.system.suggester(['Bookmark entry', 'Allow list entry', 'Deny list entry'], 
    ['bookmark', 'allowlist', 'denylist'], true, `URI type for ${uri.data}`);
// prompt for description
uri.description = await tp.system.prompt(`Use case for ${uri.data}`, (tp.frontmatter?.metadata?.description ? 
    tp.frontmatter?.metadata?.description?.replace(/\\/g, '\\') : 
    `URI page for [${uri.label}](${uri.data}) which is a ${uri.type} entry`), true);
%>
uri: 
    data    : <%* tR += `&uri.data ${uri.data}`; %>
    label   : <% uri.label %>
    path    : <% uri.path %>
    type    : <% uri.type %>
<%*
tR += (await tp.file.include('[[📦 block~yaml ✉]]'))
    // remove yaml directives
    .replace(/^---/gm, '')
    // replace aliases
    .replace(/^((?:[ ]+)?aliases(?:[ ]+)?:)(?:[ ]+)?(.*)$/gm, 
        `$1 [${'$2, ' ?? ''}*uri.data, "${uri.type}~${uri.label}"]`)
    // replace tags
    .replace(/^((?:[ ]+)?tags(?:[ ]+)?:)(?:[ ]+)?$/gm, 
        `$1 [${(await tp.system.prompt('Tags', tp.frontmatter?.tags?.join(', ') ?? 
        'Obsidian/uri', true))?.replace(/\s+/g,'').trim().split(',').join(', ')}]`)
    // preserve existing uuid
    .replace(/^((?:[ ]+)?uuid(?:[ ]+)?:)(?:[ ]+)?(.*)$/gm, 
        `$1 ${tp.frontmatter?.uuid ? `&uuid ${tp.frontmatter?.uuid}` : '$2'}`)
    // replace metadata.description
    .replace(/^((?:[ ]+)?description(?:[ ]+)?:)(?:[ ]+)?$/gm, 
        `$1 "${uri.description}"`)
    // replace metadata.title
    .replace(/^((?:[ ]+)?title(?:[ ]+)?:)(?:[ ]+)?.*$/gm, 
        `$1 uri~${uri.data}`)
    // replace metadata.type
    .replace(/^((?:[ ]+)?type(?:[ ]+)?:)(?:[ ]+)?$/gm, 
        `$1 ${parsedFileName?.type ?? tp.config.template_file.basename
        .replace(/[^\x00-\x7F]+/g,'')
        .replace(/new/, '')
        .toLowerCase().trim().split('~')[0]}`)
    .replace(/^((?:[ ]+)?scope(?:[ ]+)?:)(?:[ ]+)?$/gm, 
        `$1 ${parsedFileName?.scope ?? '$1'}`)
    .trim();
%>

---

<% await tp.file.include('[[📦 block~header 🔝]]') %>

---

## `fas:Link` Links <% uri.data %>

```query
/<%*
// remove illegal characters from uri for rename
const uniquefileName = `uri~${uri.data
    .replace(/^[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+/g, '')
    .replace(/[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+$/g, '')
    .replace(/[\:\?\'\"\[\]\*\/\\\+\<\<\>\>\|\|]+/g, '_')}`;
// rename file
await tp.file.rename(uniquefileName);
// output #Obsidian/query/regex for uri
tR += (parsedUri.subdomain === `${parsedUri.domain ?? ''}.${parsedUri.tld ?? ''}` ?
    `${parsedUri.scheme ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&').replace(/^https?/gi, 'https?') + `(www\\.)?` + `${parsedUri.subdomain ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&') + `${parsedUri.path ?? ''}`.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&') : uri.data.replace(/[.*+?^${}()|[\]\\/]/g, '\\$&')) + '(?:\\/.*)?';
%>/ -file:(<%* tR += uniquefileName; %>)
```

## Inbox

<%-_
/_

# template

## meta~data

```dataviewfield
aliases     :: uri, new bookmark entry, new allowlist entry, new denylist entry
created     :: 2021-07-28T03:20:09-04:00
description :: uri entries (type: bookmarks, allowlist, denylist)
publish     :: true
requires    :: obsidian-auto-link-title, dataview, templater-obsidian
scope       ::
tags        :: #Obsidian/template/note/uri, #Obsidian/plugin/obsidian-auto-link-title, #Obsidian/plugin/dataview, #Obsidian/plugin/templater-obsidian
title       :: 📄 note~uri 🔗
type        :: template~note~uri
uuid        :: 019a0e62-927f-4e5b-a119-2e51dd47199e
version     :: 1
```

## meta~todo

- [x] #Obsidian/template/note/uri/refactor get rid of spaghetti code (links to [[global]]) #TODO
- [x] #Obsidian/template/note/uri/refactor remove custom resolution white space #TODO
- [ ] #Obsidian/template/note/uri/regex match metadata.<subproperly> properly to avoid mismatching #TODO
- [ ] #Obsidian/template/note/uri/rename check for existing file, cancel generation, delete file and then open existing file? #TODO
- [ ] #Obsidian/template/note/uri/refactor right now, we're only handling URLs - URN need to be supported #TODO

## meta~notes

- works on mobile

## meta~inbox

\*/
\_%>
