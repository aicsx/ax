---
layout: post
title: "VIM: copy to clipboard on wayland"
date: 2025-05-31 01:16:21 +0200
categories: blog
---

# VIM: copy to clipboard on wayland
> I had trouble finding resources so here is what ended up working by adding in ~/.vimrc.

{% highlight bash %}
~$ vim .vimrc
{% endhighlight %}

```bash
xnoremap <silent> <C-c> :w !wl-copy<CR><CR>
```

> This line add key "Control + C" for copy to clipboard 
>
> NOTE: Before you copy the content to the clipboard, you may want to select some specific lines or maybe all of them. The command is to be used after line selection in VISUAL mode.
>
>     xnoremap: mapping will work in visual mode only
>     <silent>: mapping which will not be echoed on the command line
>     <C-c>: desired key combination
>     :w !{cmd}: write the range to the stdin of cmd
>     <CR><CR>: two enters are needed otherwise command line waits for another command

```bash
:wq
```

THIS IS! 

---

(END)
