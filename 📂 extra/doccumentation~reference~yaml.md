---
# for [[software~Obsidian~plugin~obsidian-advanced-uri]]
uuid    : &uuid 1ffc82bf-b446-436a-89f1-1e1b8596a0e4
# built-in [[software~Obsidian#yaml]]
aliases : [*uuid, YAML demo, YAML documentation, YAML examples, YAML reference]
cssclass: 
publish : 
tags    : [yaml, specification, reference, NOTE/OBJECT/documentation, NOTE/OBJECT/demo, Obsidian/component/yaml]
# for [[software~Obsidian~plugin~breadcrumbs]]
conatiner   : 
near        : 
contains    : 
# this note
metadata:
    datetime: 
        created : 2021-08-06T17:29:58-04:00
        # from [[software~Obsidian~plugin~obsidian-juliandate]]
        julian  : 2459433.22915
    description : "`yaml` examples (see metadata) and spec (iframe) from [YAML Ain’t Markup Language (YAML™) Version 1.2](https://yaml.org/spec/1.2/spec.html)"
    scope       : personal
    status      :
        archived: false
        reviewed: false
    title       : YAML Examples and Documentation
    type        : documentation~reference
# this note's template
source:
    publish : true
  # target  : "[[📂 nonhierarchical/yaml.md]]"
    template: "[[📁 templates/📦 block~yaml ✉.md]]"
    version : 1
yaml:
    version         : 1.2
    specification   : https://yaml.org/spec/1.2/spec.html
    # YAML DEMO [YAML Ain’t Markup Language (YAML™) Version 1.2](https://yaml.org/spec/1.2/spec.html)
    2.1.: # Example 2.1.  Sequence of Scalars
    # (ball players) 
    - Mark McGwire  
    - Sammy Sosa
    - Ken Griffey
    2.2.: # Example 2.2.  Mapping Scalars to Scalars
        # (player statistics)
        hr:  65    # Home runs  
        avg: 0.278 # Batting average
        rbi: 147   # Runs Batted In
    2.3.: # Example 2.3.  Mapping Scalars to Sequences
        # (ball clubs in each league) 
        american:
          - Boston Red Sox
          - Detroit Tigers
          - New York Yankees
        national:
          - New York Mets
          - Chicago Cubs
          - Atlanta Braves
    2.4.: # Example 2.4.  Sequence of Mappings
    # (players’ statistics) 
    -
      name: Mark McGwire
      hr:   65
      avg:  0.278
    -
      name: Sammy Sosa
      hr:   63
      avg:  0.288
    2.5.: # Example 2.5. Sequence of Sequences
    - [name        , hr, avg  ]
    - [Mark McGwire, 65, 0.278]
    - [Sammy Sosa  , 63, 0.288]
    2.6.: # Example 2.6. Mapping of Mappings
        Mark McGwire: {hr: 65, avg: 0.278}
        Sammy Sosa: {
            hr: 63,
            avg: 0.288
          }
    2.9.: # Example 2.9.  Single Document with
        # Two Comments 
        hr: # 1998 hr ranking
          - Mark McGwire
          - Sammy Sosa
        rbi:
          # 1998 rbi ranking
          - Sammy Sosa
          - Ken Griffey
    2.10.: # Example 2.10.  Node for “Sammy Sosa”
        #  appears twice in this document 
        hr:
          - Mark McGwire
          # Following node labeled SS
          - &SS Sammy Sosa
        rbi:
          - *SS # Subsequent occurrence
          - Ken Griffey
    2.11.: # Example 2.11. Mapping between Sequences
        ? - Detroit Tigers
          - Chicago cubs
        :
          - 2001-07-23

        ? [ New York Yankees,
            Atlanta Braves ]
        : [ 2001-07-02, 2001-08-12,
            2001-08-14 ]
    2.12.: # Example 2.12. Compact Nested Mapping
        # Products purchased
        - item    : Super Hoop
          quantity: 1
        - item    : Basketball
          quantity: 4
        - item    : Big Shoes
          quantity: 1
    2.13.: # Example 2.13.  In literals,
        # newlines are preserved 
        # ASCII Art
        |
          \//||\/||
          // ||  ||__
    2.14.: # Example 2.14.  In the folded scalars,
        # newlines become spaces 
        >
          Mark McGwire's
          year was crippled
          by a knee injury.
    2.15.: # Example 2.15.  Folded newlines are preserved
        # for "more indented" and blank lines 
        >
         Sammy Sosa completed another
         fine season with great stats.

           63 Home Runs
           0.288 Batting Average

         What a year!
    2.16.: # Example 2.16.  Indentation determines scope
        name: Mark McGwire
        accomplishment: >
          Mark set a major league
          home run record in 1998.
        stats: |
          65 Home Runs
          0.278 Batting Average
    2.17: # Example 2.17. Quoted Scalars
        unicode: "Sosa did fine.\u263A"
        control: "\b1998\t1999\t2000\n"
        hex esc: "\x0d\x0a is \r\n"

        single: '"Howdy!" he cried.'
        quoted: ' # Not a ''comment''.'
        tie-fighter: '|\-*-/|' 
    2.18.: # Example 2.18. Multi-line Flow Scalars
        plain:
          This unquoted scalar
          spans many lines.

        quoted: "So does this
          quoted scalar.\n"
    2.19.: # Example 2.19. Integers
        canonical: 12345
        decimal: +12345
        octal: 0o14
        hexadecimal: 0xC
    2.20.: # Example 2.20. Floating Point
        canonical: 1.23015e+3
        exponential: 12.3015e+02
        fixed: 1230.15
        negative infinity: -.inf
        not a number: .NaN
    2.21.: # Example 2.21. Miscellaneous
        null:
        label: null
        booleans: [ true, false ]
        string: '012345'
    2.22.: # Example 2.22. Timestamps
        canonical: 2001-12-15T02:59:43.1Z
        iso8601: 2001-12-14t21:59:43.10-05:00
        spaced: 2001-12-14 21:59:43.10 -5
        date: 2002-12-14
    2.23.: # Example 2.23. Various Explicit Tags
        not-date: !!str 2002-04-28

        picture: !!binary |
         R0lGODlhDAAMAIQAAP//9/X
         17unp5WZmZgAAAOfn515eXv
         Pz7Y6OjuDg4J+fn5OTk6enp
         56enmleECcgggoBADs=

        application specific tag: !something |
         The semantics of the tag
         above may be different for
         different documents.
    2.24.: # Example 2.24. Global Tags
        %TAG ! tag:clarkevans.com,2002:
            !shape
              # Use the ! handle for presenting
              # tag:clarkevans.com,2002:circle
            - !circle
              center: &ORIGIN {x: 73, y: 129}
              radius: 7
            - !line
              start: *ORIGIN
              finish: { x: 89, y: 102 }
            - !label
              start: *ORIGIN
              color: 0xFFEEBB
              text: Pretty vector drawing.
    2.25.: # Example 2.25. Unordered Sets
        # Sets are represented as a
        # Mapping where each key is
        # associated with a null value 
        !!set
        ? Mark McGwire
        ? Sammy Sosa
        ? Ken Griff
    2.26.: # Example 2.26. Ordered Mappings
        # Ordered maps are represented as
        # A sequence of mappings, with
        # each mapping having one key
        !!omap
        - Mark McGwire: 65
        - Sammy Sosa: 63
        - Ken Griffy: 58
    2.27.: # Example 2.27. Invoice
        !<tag:clarkevans.com,2002:invoice>
        invoice: 34843
        date   : 2001-01-23
        bill-to: &id001
            given  : Chris
            family : Dumars
            address:
                lines: |
                    458 Walkman Dr.
                    Suite #292
                city    : Royal Oak
                state   : MI
                postal  : 48046
        ship-to: *id001
        product:
            - sku         : BL394D
              quantity    : 4
              description : Basketball
              price       : 450.00
            - sku         : BL4438H
              quantity    : 1
              description : Super Hoop
              price       : 2392.00
        tax  : 251.42
        total: 4443.52
        comments:
            Late afternoon is best.
            Backup contact is Nancy
            Billsmer @ 338-4338.
    2.28.: # Example 2.28. Log File
        -
            Time: 2001-11-23 15:01:42 -5
            User: ed
            Warning:
              This is an error message
              for the log file
        -
            Time: 2001-11-23 15:02:31 -5
            User: ed
            Warning:
              A slightly different error
              message.
        -
            Date: 2001-11-23 15:03:17 -5
            User: ed
            Fatal:
              Unknown variable "bar"
            Stack:
              - file: TopClass.py
                line: 23
                code: |
                  x = MoreObject("345\n")
              - file: MoreClass.py
                line: 58
                code: |-
                  foo = bar
    5.3.: # Example 5.3. Block Structure Indicators 
        sequence:
        - one
        - two
        mapping:
          ? sky
          : blue
          sea : green
    5.3.1.: # %YAML 1.2
        !!map {
          ? !!str "sequence"
          : !!seq [ !!str "one", !!str "two" ],
          ? !!str "mapping"
          : !!map {
            ? !!str "sky" : !!str "blue",
            ? !!str "sea" : !!str "green",
          },
        }
    5.4.: # Example 5.4. Flow Collection Indicators 
        sequence: [ one, two, ]
        mapping: { sky: blue, sea: green }
    5.4.1.: # %YAML 1.2
        !!map {
          ? !!str "sequence"
          : !!seq [ !!str "one", !!str "two" ],
          ? !!str "mapping"
          : !!map {
            ? !!str "sky" : !!str "blue",
            ? !!str "sea" : !!str "green",
          },
        }
    5.6.: # Example 5.6. Node Property Indicators 
        anchored: !local &anchor value
        alias: *anchor
    5.6.1.: # %YAML 1.2
        !!map {
          ? !!str "anchored"
          : !local &A1 "value",
          ? !!str "alias"
          : *A1,
        }
    5.7.: # Example 5.7. Block Scalar Indicators 
        literal: |
          some
          text
        folded: >
          some
          text
    5.7.1: # %YAML 1.2
        !!map {
          ? !!str "literal"
          : !!str "some\ntext\n",
          ? !!str "folded"
          : !!str "some text\n",
        }
    5.8.: # Example 5.8. Quoted Scalar Indicators 
        single: 'text'
        double: "text"
    5.8.1.: # %YAML 1.2
        !!map {
          ? !!str "single"
          : !!str "text",
          ? !!str "double"
          : !!str "text",
        }
---

# `=this.metadata.title`

**`=this.metadata.description`**, modified: _`$=moment(dv.current().file.mtime.toString()).format('LLLL')`_

## Related

- [[test-meta-obsidian]]

# Source

<iframe 
    border=1
    frameborder=1
    height=1000
    width=900
    src="https://yaml.org/spec/1.2/spec.html">
</iframe>

## `fas:Comment` Mentions yaml

```query
"yaml" -file:(yaml)
```