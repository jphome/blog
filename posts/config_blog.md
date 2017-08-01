---
title: 配置blog环境
date: '2013-10-14 21:54:42'
permalink: 
description: 
categories: 
- 技术相关

tags: 
- blog
---


### 配置vim(~/.vimrc)
#### 自动添加mkd文件头
##### 1.新建文件(vim xxx.md)
    autocmd BufNewFile *.md exec ":call SetTitle()"
    func! SetTitle()
        if &filetype == 'mkd'
            call setline(1,"---")
            call append(line("."), "title: ")
    		call append(line(".")+1, "date: '".strftime("%Y-%m-%d %H:%M:%S")."'")
    		call append(line(".")+2, "permalink: ")
    		call append(line(".")+3, "description: ")
    		call append(line(".")+4, "categories: ")
    		call append(line(".")+5, "- ")
    		call append(line(".")+6, "- ")
    		call append(line(".")+7, "")
    		call append(line(".")+8, "tags: ")
    		call append(line(".")+9, "- ")
    		call append(line(".")+10, "---")
    		call append(line(".")+11, "")
    		call append(line(".")+12, "")
        endif
    endfunc
	

#### 2.更新已有文件(xxx -> xxx.md)
    map <F5> :call AddAutoHead()<CR>
    func! AddAutoHead()
        exec "w"
        if &filetype == 'mkd'
            call append(0, "---")
            call append(1, "title: ")
            call append(2, "date: '".strftime("%Y-%m-%d %H:%M:%S")."'")
            call append(3, "permalink: ")
            call append(4, "description: ")
            call append(5, "categories: ")
            call append(6, "- ")
            call append(7, "- ")
            call append(8, "")
            call append(9, "tags:")
            call append(10, "- ")
            call append(11, "---")
            call append(12, "")
            call append(13, "")
            endif
    endfunc


### 自动更新当前编辑文件至blog(本地) [按Ctrl+F5]
    " 需要事先设置blog_root环境变量
    map <C-F5> :call RunXxx()<CR>
    func! RunXxx()
    	exec "w"
        if &filetype == 'mkd'
    		exec "!cp % $blog_root/posts"
    		exec "!cd $blog_root && gor compile"
    	endif
    endfunc

