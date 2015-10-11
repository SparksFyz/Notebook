## sublime install&plugin&config

### Install package control

- show console (Ctrl+`)

- paste the appropriate Python code for your version of Sublime Text into the console.
    - sublime text 3 :

```shell

import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

### My plugin

1. Emmet
2. JsFormat
3. BracketHighlighter
4. Markdown Extend
5. Markdown Preview
6. Color theme : Monokai extend
7. Theme : Soda
8. gitGutter
9. knockdown
10. sublime linter
11. SideBarEnhancements
12. [Table Editor](https://packagecontrol.io/packages/Table%20Editor)

### config

```
{
    "align_indent": false,
    "alignment_chars":
    [
        "=",
        ":"
    ],
    "alignment_space_chars":
    [
        "=",
        ":"
    ],
    "caret_style": "phase",
    "color_scheme": "Packages/Monokai Extended/Monokai Extended.tmTheme",
    "font_options":
    [
        "subpixel_antialias"
    ],
    "font_size": 15,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "rulers":
    [
        119
    ],
    "scroll_past_end": true,
    "tab_size": 2,
    "theme": "Soda Dark.sublime-theme",
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "wrap_width": 119
}


```




