---
title: SED
date: 2023-05-12
tags: [sed, linux, cli, snippet]
---

# SED
---
**sed** ("stream editor") is a **Linux ([[linux]])**, and Unix utility that parses and transforms text, using a simple, compact programming language.

## Usage

TMP
replace pattern: 
```bash
sed -i 's/Steven/Kate/g' file
```

- **-i** : Edit files in-place but treat each file independently
- **s/** : Search file for a word
- **g/** : Replace with word globally
