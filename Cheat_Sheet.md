#Cheat Sheet

##Headings

Heading 1	# Heading 1
Heading 2	## Heading 2
Heading 3	### Heading 3
Heading 4	#### Heading 4
Heading 5	##### Heading 5
Heading 6	###### Heading 6

Text	
Indent size	equal to 4 spaces
bold	**bold** or __bold__
Italic	*italic* or _italic_
Strikethrough	~~strikethrough~~
Escape character	\escaped character
Bullet	-

Links	
Internal link	[text to display on link](#heading-to-link-to "optional link title") (must be all lower-case, alphanumeric and separated by hyphens)
External link	[External link](URL "Optional link title")
Relative link	[text to display](../folder/file.htm "optional link title")
Help Link	help[help text here](https://URL.com)
Hint Link	hint[hint text here](https://URL.com)
Knowledge Link	knowledge[knowledge text here](https://URL.com)

Link Behavior Prefixes:	
Add a behavior prefix to control the way the link opens	
Open in portal window	< before the link
Open in a dialog	^ before the link
Open in new window	no special characters before the link

Page	
Page break	===
Horizontal Line	--- or *** or ___
Block quote	> text to display in block quote

Embedded Content	
Image	![text to display](URL)
Video	!video[text to display](URL) (URLs from YouTube.com auto embed)
Audio	!audio[text to display](URL)
Image with link	[![image description](URL of image "image description")](URL to open when image is clicked)
Portal Link	<[text to display](URL)
Image Link	image[text to display](URL)
Video Link	video[text to display](URL)
Image Dimensions	![](image url){widthXheight} or {width} (height will be calculated automatically)

Special	
Include	!instructions[](url)
Copyable Text	++copyable text++
Type Text	+++Type text+++
Copyable and Type Text	++++Copyable and Type text++++
Replacement Token	click the @ lab toolbar button
Embed YouTube video	!video[text to display](url) (URLs from YouTube.com auto embed)
Knowledge Block	>[!knowledge] Knowledge blocks help students learn more
Alert Block	>[!alert] Alert blocks draw attention to important issues!
Note Block	>[!note]
Help Block	>[!help]
Hint Block	>[!hint]
Expandable Block:

>[+] The title is here
> 
> More of your summary goes here.
Expandable Alert Block:

>[+alert] Your alert text here.
> 
> More of your alert goes here.
Expandable Hint Block:

>[+hint] hint text here.
> 
> More of your hint goes here.
Expandable Help Block:

>[+help] help text here.
> 
> More of your help goes here.
Expandable Note Block:

>[+NOTE] note text here.
> 
> More of your note goes here.
Sections

:::sectionNamevariableName=variableValue
section text or markdown elements
:::
Variables

Define the variable

@lab.TextBox(name)
Callback the variable

@lab.Variable(name)
Inline Code

Inline code `code`

Code Blocks

Fenced_code_block
```PowerShell
get-service | stop-service -whatif
```
Code Block Modifiers

No code highlighting, copyable

```PowerShell-nocolor
Code Block
```
No tab on code block, code highlighted, copyable

```PowerShell-notab
Code Block
```
No code highlighting, no tab, not copyable

```PowerShell-nocode
Code Block
```
Code highlighted, not copyable

```PowerShell-nocopy
Code Block
```
Code_highlighted, copyable, multi line command wraps to the next line

```PowerShell-wrap
Code Block
```
Code_highlighted, copyable, line numbers added to each line

```PowerShell-linenums
Code Block
```
Reference Instruction Block

!INSTRUCTIONS[][reference label]

> [reference label]:
> These instructions are injected in the statement above.
Dialog Windows

Dialog

^[text to display in lab  instructions][Reference Link]

> [Reference Link]:
> This text will appear in the Dialog popup
Instruction Dialog

> ^INSTRUCTIONS[text](url)
Commands (requires Integration Services to be installed on the VM)

Single Line

@[text to display](`command`)
Multi-Line

@[text to display][multi-line-command-id]

[multi-line-command-id]:
```
Multi-line
Command-goes-here
```
Reference links

Text_lookup
[Reference link]
[Reference link]: URL "Optional link title"
Label_lookup
[Reference link][Name of URL]
[Name of URL]: URL "Optional link title"
Footnote_style
[Reference link][1]
[1]: URL "Optional link title"
Tasks

Unordered_Task_List
- [] Item 1
- [] Item 2
- [] Item 3
- [] Item 4
- [] Item 5
Ordered_Task_List
1. [] Item 1
1. [] Item 2
1. [] Item 3
1. [] Item 4
1. [] Item 5
Sample table

Left aligned text

| column 1 | column 2 |
|:---------|:---------|
| data 1   | data 2   |
| data 3   | data 4   |
Right aligned text

| column 1 | column 2 |
|---------:|---------:|
| data 1   | data 2   |
| data 3   | data 4   |
Center aligned text

| column 1 | column 2 |
|:--------:|:--------:|
| data 1   | data 2   |
| data 3   | data 4   |
