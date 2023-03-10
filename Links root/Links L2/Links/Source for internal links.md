## Wikilink by name

### There's not difference if target page have alias if linking by its name:

`[[Target without aliases]]` => [[Target without aliases]]
`[[Target with aliases]]` => [[Target with aliases]]

---

### Also page can be linked by relative or full path to it

`[[Links/Target with aliases]]` => [[Links/Target with aliases]]

`[[Links root/Links L2/Links/Target without aliases]]` => [[Links root/Links L2/Links/Target without aliases]]

`[[/Links root/Links L2/Links/Target without aliases]]` => [[/Links root/Links L2/Links/Target without aliases]]

> Following example incorrect, because there is no `/Links` folder in the root
> of obsidian vault. Following this link will lead to create new page instead open existing one.
> 
> `[[/Links/Target with aliases]]` => [[/Links/Target without aliases]] 

---

### And this method can be used to link page with a same name, which in different folder

`[[Links duplicate/Target without aliases]]` => [[Links duplicate/Target without aliases]]

`[[Links root/Links L2/Links duplicate/Target without aliases]]` => [[Links root/Links L2/Links duplicate/Target without aliases]]

`[[/Links root/Links L2/Links duplicate/Target without aliases]]` => [[/Links root/Links L2/Links duplicate/Target without aliases]]

---

### What if there's page with a same name in the parent folder and in the same folder

Will be linked to the page in the same folder:
`[[Target without aliases]]` => [[Target without aliases]]

---

### What if there's page with a same name in the parent folder and in the grandparent folder 

There're couple of pages:
1. /Links root/Target parent
2. /Links root/Links L2/Target parent

Will be linked to the first page relative to the vault root(1st variant in the current case)

`[[Target parent]]` => [[Target parent]]

---

### What if there's page with a same name in the parent, grandparent and child folders

There are couple of pages:
1. /Links root/Target child
2. /Links root/Links L2/Target child
3. /Links root/Links L2/Links/Links L4/Target child

Following format will be linked to page in child folder(3rd variant)

`[[Target child]]` => [[Target child]]

---

### What if there's page with a same name in the parent, grandparent and child folders but path is relative

There are couple of pages:
1. /Links root/Target child
2. /Links root/Links L2/Target child
3. /Links root/Links L2/Links/Links L4/Target child

`[[Links L2/Target child]]` => [[Links L2/Target child]]

---

### What if same combination of folder/file but in the different levels of vault tree

There're couple of pages:
1. /Links root/Links sibling/Target relative path
2. /Links root/Links L2/Links sibling/Target relative path
3. /Links root/Links L2/Links/Links sibling/Target relative path

2nd variant. WTF???

`[[Links sibling/Target relative path]]` => [[Links sibling/Target relative path]]

---
### Algorithm summary

If specified only page name(no `/` in the link):
1. Search in the current folder
2. Search in the children folders
3. Search from root

If specified path from root(starting from: `/`)
1. Search just by path relative to the root of vault

If specified relative path:
1. WTF?

## Wikilink by alias

### Link by filename and  exsting single alias

`[[Target with aliases|TWA1]]` => [[Target with aliases|TWA1]]

---

### Link by filename and multiple existing alias

`[[Target with aliases|TWA1|TWA2]]` => [[Target with aliases|TWA1|TWA2]]

---

### Link by filename and unexisting alias

`[[Target with aliases|TWNOTEXISTS]]` => [[Target with aliases|TWNOTEXISTS]] 

---

### Link by only existing alias

Doesn't work, will lead to creating new page
`[[TWA]]` => [[TWA]]

----

### Link by filepath and existing alias

`[[Links duplicate/Target with aliases|DTWA1]]` => [[Links duplicate/Target with aliases|DTWA1]]

`[[Links root/Links L2/Links duplicate/Target with aliases|DTWA1]]` => [[Links root/Links L2/Links duplicate/Target with aliases|DTWA1]]

`[[/Links root/Links L2/Links duplicate/Target with aliases|DTWA1]]` => [[/Links root/Links L2/Links duplicate/Target with aliases|DTWA1]]

---
### Algorithm summary

1. Algorithm of searching file the same as for links without aliases
2. All symbols after `|` should be considered as link name


## Markdown link

### All options bellow are correct:

`[Link name](/Links%20root/Links%20L2/Links/Target%20without%20aliases.md)` => [Link name](/Links%20root/Links%20L2/Links/Target%20without%20aliases.md)

`[Link name](/Links%20root/Links%20L2/Links/Target%20without%20aliases)` => [Link name](/Links%20root/Links%20L2/Links/Target%20without%20aliases)

`[Link name](Links%20root/Links%20L2/Links/Target%20without%20aliases)` => [Link name](Links%20root/Links%20L2/Links/Target%20without%20aliases)

`[Link name](Target%20without%20aliases)` => [Link name](Links%20root/Links%20L2/Links/Target%20without%20aliases)

`[Link name](Random%20page)` => [Link name](Random%20page)

### Algorithm summary

Rules for search file the same as for wikilinks

## Overall algorithm

Search file algorithm:

If specified only page name(no `/` in the link):
1. Search in the current folder
2. Search in the children folders
3. Search from root

If specified path from root(starting from: `/`)
1. Search just by path relative to the root of vault

If specified relative path:
1. WTF? first match?

Match rules:
1. if specified extension - search full match, files: \*.md and \*.markdown will be treated as different files
2. if extension isn't specified - search only by file base name, files: \*.md and \*.markdown will be treated as same files

Naming rules:
1. If wikilink and alias specified -> link name all symbols after first `|`
2. if wikilink and alias isn't specified -> link name eq wikilink
3. if markdown link -> link name from `[]`