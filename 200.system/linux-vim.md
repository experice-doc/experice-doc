# vim 配置 (linux mac 配置)
### vim 注释和删除注释
```
在10 - 20行添加 // 注释 
:10,50s#^#//#g 
在10 - 20行删除 // 注释 
:10,20s#^//##g 
  
在10 - 20行添加 # 注释 
:10,20s/^/#/g 
在10 - 20行删除 # 注释 
:10,20s/^#//g 
```

### 首先要知道三个配置文件：vimrc，gvimrc和exrc

#### vimrc
```
vimrc是Vim最主要的配置文件，它有两个版本：全局版本（global）和用户版本（personal）。全局vimrc文件在Vim的安装目录中，我的电脑是Mac，所以其路径是
    /usr/share/vim/vimrc

假如你不知道全局vimrc的位置，可以打开Vim，在普通模式（Normal）下输入下面的命令得到它的位置：
echo  $VIM（注意大小写）
       用户版本的vimrc文件在当前用户的主目录下，主目录的位置依赖于操作系统。Mac下的用户vimrc文件路径为：
/Users/用户名/.vimrc（文件名前面的”.”代表这个文件是隐藏文件）
       你可以在Vim的普通模式下输入下面的命令，查找用户主目录的位置：
:echo  $HOME
       但是Mac下默认是没有用户vimrc的，所以需要你自己创建一个。
       不管怎么改用户版的vimrc文件，其中的内容都是是覆盖在全局vimrc文件中设置的内容，这就意味着你可以不需要去改变全局vimrc文件来进行配置vim，只需要修改用户vimrc文件。
```

#### gvimrc
```
gvimrc文件是Gvim的配置文件，和vimrc很相似，并且是放在同一个目录下的，也分为全局版和用户版。这个文件是用来设置只有Gvim才能使用的GUI设置。我感觉Vim比Gvim好用，所以没有管这个文件。
```

#### exrc
```
exrc文件是用作与vi或ex向后兼容的，它也和vimrc放在同一个目录，当然也分全局版和用户版。然而，除非你想用vi兼容的模式来使用Vim，否则你更本不会用到这个文件。当然一般人都不会用vi兼容模式来使用Vim的。
```

#### 然后是配置自己喜欢的Vim。
```
1. cd ~ 到根目录。
2. touch .vimrc
3. 接下来打开用户vimrc文件并在里面添加各种Vim命令。将我的Vim文件复制到下面：
```

##### .vimrc 配置 (本人只用了一些基本配置)
```
" Configuration file for vim
set modelines=0" CVE-2007-2438

" Normally we use vim-extensions. If you want true vi-compatibility
" remove change the following statements
set nocompatible" Use Vim defaults instead of 100% vi compatibility
set backspace=2" more powerful backspacing

set nocompatible " 关闭 vi 兼容模式
syntax on " 自动语法高亮
colorscheme darkblue "desert molokai  设定配色方案
set number " 显示行号
set cursorline " 突出显示当前行
set ruler " 打开状态栏标尺
set shiftwidth=4 " 设定 << 和 >> 命令移动时的宽度为 4
set softtabstop=4 " 使得按退格键时可以一次删掉 4 个空格
set tabstop=4 " 设定 tab 长度为 4
set nobackup " 覆盖文件时不备份
set autochdir " 自动切换当前目录为当前文件所在的目录
filetype plugin indent on " 开启插件
set backupcopy=yes " 设置备份时的行为为覆盖
set ignorecase smartcase " 搜索时忽略大小写，但在有一个或以上大写字母时仍保持对大小写敏感
set nowrapscan " 禁止在搜索到文件两端时重新搜索
set incsearch " 输入搜索内容时就显示搜索结果
set hlsearch " 搜索时高亮显示被找到的文本
set noerrorbells " 关闭错误信息响铃
set novisualbell " 关闭使用可视响铃代替呼叫
set t_vb= " 置空错误铃声的终端代码
" set showmatch " 插入括号时，短暂地跳转到匹配的对应括号
" set matchtime=2 " 短暂跳转到匹配括号的时间
set magic " 设置魔术
set hidden " 允许在有未保存的修改时切换缓冲区，此时的修改由 vim 负责保存
set guioptions-=T " 隐藏工具栏
set guioptions-=m " 隐藏菜单栏
set smartindent " 开启新行时使用智能自动缩进
set backspace=indent,eol,start
" 不设定在插入状态无法用退格键和 Delete 键删除回车符
set cmdheight=1 " 设定命令行的行数为 1
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\ 
" 设置在状态行显示的信息
set foldenable " 开始折叠
set foldmethod=syntax " 设置语法折叠
set foldcolumn=0 " 设置折叠区域的宽度
setlocal foldlevel=1 " 设置折叠层数为
" set foldclose=all " 设置为自动关闭折叠 
" nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR>
" 用空格键来开关折叠


" return OS type, eg: windows, or linux, mac, et.st..
function! MySys()
if has("win16") || has("win32") || has("win64") || has("win95")
return "windows"
elseif has("unix")
return "linux"
endif
endfunction


" 用户目录变量$VIMFILES
if MySys() == "windows"
let $VIMFILES = $VIM.'/vimfiles'
elseif MySys() == "linux"
let $VIMFILES = $HOME.'/.vim'
endif


" 设定doc文档目录
let helptags=$VIMFILES.'/doc'


" 设置字体 以及中文支持
if has("win32")
set guifont=Inconsolata:h12:cANSI
endif


" 配置多语言环境
if has("multi_byte")
" UTF-8 编码
set encoding=utf-8
set termencoding=utf-8
set formatoptions+=mM
set fencs=utf-8,gbk


if v:lang =~? '^\(zh\)\|\(ja\)\|\(ko\)'
set ambiwidth=double
endif


if has("win32")
source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim
language messages zh_CN.utf-8
endif
else
echoerr "Sorry, this version of (g)vim was not compiled with +multi_byte"
endif


" Buffers操作快捷方式!
nnoremap <C-RETURN> :bnext<CR>
nnoremap <C-S-RETURN> :bprevious<CR>


" Tab操作快捷方式!
nnoremap <C-TAB> :tabnext<CR>
nnoremap <C-S-TAB> :tabprev<CR>


"关于tab的快捷键
" map tn :tabnext<cr>
" map tp :tabprevious<cr>
" map td :tabnew .<cr>
" map te :tabedit
" map tc :tabclose<cr>


"窗口分割时,进行切换的按键热键需要连接两次,比如从下方窗口移动
"光标到上方窗口,需要<c-w><c-w>k,非常麻烦,现在重映射为<c-k>,切换的
"时候会变得非常方便.
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l


"一些不错的映射转换语法（如果在一个文件中混合了不同语言时有用）
nnoremap <leader>1 :set filetype=xhtml<CR>
nnoremap <leader>2 :set filetype=css<CR>
nnoremap <leader>3 :set filetype=javascript<CR>
nnoremap <leader>4 :set filetype=php<CR>


" set fileformats=unix,dos,mac
" nmap <leader>fd :se fileformat=dos<CR>
" nmap <leader>fu :se fileformat=unix<CR>


" use Ctrl+[l|n|p|cc] to list|next|previous|jump to count the result
" map <C-x>l <ESC>:cl<CR>
" map <C-x>n <ESC>:cn<CR>
" map <C-x>p <ESC>:cp<CR>
" map <C-x>c <ESC>:cc<CR>


" 让 Tohtml 产生有 CSS 语法的 html
" syntax/2html.vim，可以用:runtime! syntax/2html.vim
let html_use_css=1


" Python 文件的一般设置，比如不要 tab 等
autocmd FileType python set tabstop=4 shiftwidth=4 expandtab
autocmd FileType python map <F12> :!python %<CR>


" 选中状态下 Ctrl+c 复制
vmap <C-c> "+y


" 打开javascript折叠
let b:javascript_fold=1
" 打开javascript对dom、html和css的支持
let javascript_enable_domhtmlcss=1
" 设置字典 ~/.vim/dict/文件的路径
autocmd filetype javascript set dictionary=$VIMFILES/dict/javascript.dict
autocmd filetype css set dictionary=$VIMFILES/dict/css.dict
autocmd filetype php set dictionary=$VIMFILES/dict/php.dict


"-----------------------------------------------------------------
" plugin - bufexplorer.vim Buffers切换
" \be 全屏方式查看全部打开的文件列表
" \bv 左右方式查看 \bs 上下方式查看
"-----------------------------------------------------------------


"-----------------------------------------------------------------
" plugin - taglist.vim 查看函数列表，需要ctags程序
" F4 打开隐藏taglist窗口
"-----------------------------------------------------------------
if MySys() == "windows" " 设定windows系统中ctags程序的位置
let Tlist_Ctags_Cmd = '"'.$VIMRUNTIME.'/ctags.exe"'
elseif MySys() == "linux" " 设定windows系统中ctags程序的位置
let Tlist_Ctags_Cmd = '/usr/bin/ctags'
endif
nnoremap <silent><F4> :TlistToggle<CR>
let Tlist_Show_One_File = 1 " 不同时显示多个文件的tag，只显示当前文件的
let Tlist_Exit_OnlyWindow = 1 " 如果taglist窗口是最后一个窗口，则退出vim
let Tlist_Use_Right_Window = 1 " 在右侧窗口中显示taglist窗口
let Tlist_File_Fold_Auto_Close=1 " 自动折叠当前非编辑文件的方法列表
let Tlist_Auto_Open = 0
let Tlist_Auto_Update = 1
let Tlist_Hightlight_Tag_On_BufEnter = 1
let Tlist_Enable_Fold_Column = 0
let Tlist_Process_File_Always = 1
let Tlist_Display_Prototype = 0
let Tlist_Compact_Format = 1


"-----------------------------------------------------------------
" plugin - mark.vim 给各种tags标记不同的颜色，便于观看调式的插件。
" \m mark or unmark the word under (or before) the cursor
" \r manually input a regular expression. 用于搜索.
" \n clear this mark (i.e. the mark under the cursor), or clear all highlighted marks .
" \* 当前MarkWord的下一个 \# 当前MarkWord的上一个
" \/ 所有MarkWords的下一个 \? 所有MarkWords的上一个
"-----------------------------------------------------------------


"-----------------------------------------------------------------
" plugin - NERD_tree.vim 以树状方式浏览系统中的文件和目录
" :ERDtree 打开NERD_tree :NERDtreeClose 关闭NERD_tree
" o 打开关闭文件或者目录 t 在标签页中打开
" T 在后台标签页中打开 ! 执行此文件
" p 到上层目录 P 到根目录
" K 到第一个节点 J 到最后一个节点
" u 打开上层目录 m 显示文件系统菜单（添加、删除、移动操作）
" r 递归刷新当前目录 R 递归刷新当前根目录
"-----------------------------------------------------------------
" F3 NERDTree 切换
map <F3> :NERDTreeToggle<CR>
imap <F3> <ESC>:NERDTreeToggle<CR>


"-----------------------------------------------------------------
" plugin - NERD_commenter.vim 注释代码用的，
" [count],cc 光标以下count行逐行添加注释(7,cc)
" [count],cu 光标以下count行逐行取消注释(7,cu)
" [count],cm 光标以下count行尝试添加块注释(7,cm)
" ,cA 在行尾插入 ,并且进入插入模式。 这个命令方便写注释。
" 注：count参数可选，无则默认为选中行或当前行
"-----------------------------------------------------------------
let NERDSpaceDelims=1 " 让注释符与语句之间留一个空格
let NERDCompactSexyComs=1 " 多行注释时样子更好看


"-----------------------------------------------------------------
" plugin - DoxygenToolkit.vim 由注释生成文档，并且能够快速生成函数标准注释
"-----------------------------------------------------------------
let g:DoxygenToolkit_authorName="Asins - asinsimple AT gmail DOT com"
let g:DoxygenToolkit_briefTag_funcName="yes"
map <leader>da :DoxAuthor<CR>
map <leader>df :Dox<CR>
map <leader>db :DoxBlock<CR>
map <leader>dc a <LEFT><LEFT><LEFT>


"-----------------------------------------------------------------
" plugin – checksyntax.vim JavaScript常见语法错误检查
" 默认快捷方式为 F5
"-----------------------------------------------------------------
let g:checksyntax_auto = 0 " 不自动检查

"-----------------------------------------------------------------
" plugin - NeoComplCache.vim 自动补全插件
"-----------------------------------------------------------------
let g:AutoComplPop_NotEnableAtStartup = 1
let g:NeoComplCache_EnableAtStartup = 1
let g:NeoComplCache_SmartCase = 1
let g:NeoComplCache_TagsAutoUpdate = 1
let g:NeoComplCache_EnableInfo = 1
let g:NeoComplCache_EnableCamelCaseCompletion = 1
let g:NeoComplCache_MinSyntaxLength = 3
let g:NeoComplCache_EnableSkipCompletion = 1
let g:NeoComplCache_SkipInputTime = '0.5'
let g:NeoComplCache_SnippetsDir = $VIMFILES.'/snippets'
" <TAB> completion.
inoremap <expr><TAB> pumvisible() ? "\<C-n>" : "\<TAB>"
" snippets expand key
imap <silent> <C-e> <Plug>(neocomplcache_snippets_expand)
smap <silent> <C-e> <Plug>(neocomplcache_snippets_expand)

```

### VIM代码补全提示功能

```
vim是一款支持插件、功能无比强大的编辑器，无论你的系统是Linux、unix、mac还是windows，都能够选择他来编辑文件或是进行工程级别 的coding。如果能把vim用好了，不仅编程效率能得到大幅度提高，周围人也会因此而看得头晕眼花佩服不已，自己心里当然也会心花怒放啦。下面就让我 来介绍一下如何来进行配置。这些配置所涉及到的内容有：autocomplpop, ctags, TagList,omnicppcomplete
首 先Vim是内建代码补全功能的，在不需要通过任何设置的情况下就能使用。在您编辑代码的时候，键入 ctrl+x, ctrl+o, ctrl+n, ctrl+p 等快捷键，就会弹出智能提示的菜单。但是这仍然不满足大家的要求。大多数IDE中，只要代码输入到相应的位置，补全提示就会自动的弹出来，而vim的这种 补全还需要自己手动的来触发。那么下面就介绍一种可以自动弹出补全提示的插件 — autocomplpop
== Autocomplpop ==
首先，从http://www.vim.org/scripts/script.PHP?script_id=1879处 下载autocomplpop文件。下载的是一个zip文件，解压后会有三个文件夹，分别是autoload，doc，plugin。到Vim的根目录下，找到名字和这三个一样的文件夹。不同系统目录位置不同。(unix/linux平台在/usr/share/vim/vim71中, windows平台在安装目录的vim71目录中，fedora是/usr/share/vim/vimfiles中）。
按照文件夹对应的把里面的acp.vim和其他的什么文件都copy过去。然后重启Vim。这时候应该会有错误提示，
Error detected while processing /home/carlos/.vim/plugin/acp.vim:
line 13:
***** L9 library must be installed! *****
这是插件放出的一个错误提示，查看plugin里的acp.vim可以看到。是缺少L9 library库。这个也是需要下载的。地址在下面
http://www.vim.org/scripts/script.php?script_id=3252
下载下来，它也是一个插件形式，以同样的方式copy到Vim目录下。
安装完后就可以了。
再就是这个插件默认是没有设置php自动补全的，可以设置一个PHP函数字典，让其根据字典的内容进行自动补全。
这个是一个PHP字典，php_funclist.
编辑配置文件.vimrc,在文件后面加上下面的代码
au FileType php setlocal dict+=~/.vim/php_funclist.txt
还有每次补全都要按键很费事，所以我们加入PHP的全能提示触发命令。
php 中 一般是会在 “$”, “->”, “::” 后需要出现自动补全，在 .vimrc 中加入以下代码：
if !exists(‘g:AutoComplPop_Behavior’)
let g:AutoComplPop_Behavior = {}
let g:AutoComplPop_Behavior['php'] = []
call add(g:AutoComplPop_Behavior['php'], {
\ ‘command’ : “\\“,
\ ‘pattern’ : printf(‘−>∥::∥$\k\{%d,}$’, 0),
\ ‘repeat’ : 0,
\})
endif
这样就可以了。
注意，某些时候，可能会在第一次按下触发补全的操作符时停顿一会，这可能是因为可匹配的项目过多，Vim正在索引，过后就会快了。
—————————————————————–
细心的朋友会发现，光是利用 autocomplpop这个插件还远远达不到要求。比如说：在c++中使用.或是->访问对象或指针中的成员和函数时还无法自动弹出提示，另外， 即便是自动提示也只能提示我们在当前文档中已输入的字符串。针对这种情况，我们就需要安装ctags工具和OmniCppComplete插件。 ctags是用来对文件做标记的工具，OmniCppComplete是在c和c++语言范畴内，对上述智能补全的增强版。
== ctags ==
ctags在http://ctags.sourceforge.NET/下载源码，编译后安装。常规的标记命令为 ctags -R 。”-R”表示递归创建，也就包括源代码根目录下的所有子目录下的源程序。
== CppCompleete ==
OmniCppComplete在http://www.vim.org/scripts/script.php?script_id=1520下载。下载 好之后根据里面的doc文档进行安装和使用。
这样一来，代码补全就比较完善了。但是根据以往的经验，IDE中还有一个功能，那就是函数和变量的跳转查看。比如代码中出现
代码:
if(true){
doThis();
}
我们想知道doThis()函数是如何定义和实现的，那么如何快速的来查看呢？我们就需要安装Taglist插件
== Taglist ==
安装taglist
先下载，地址是：http://www.vim.org/scripts/script.php?script_id=273，解压出来，根据其中的doc文档进行安装和配置，我是复制到.vim目录下
cp -r Downloads/taglist/* .vim
注意：前面的目录根据自己的情况而定
这就算插件安装完了，接着修改配置文件.vimrc
[#]sudo vi .vimrc
//在文件中加入下面两句
let Tlist_Show_One_File=1
let Tlist_Exit_OnlyWindow=1
现在就可以使用了，比如一个程序的目录是/var/www/wbtie/
在终端中
cd /var/www/wbtie
ctags -R
这时候在目录下会生成一个tags文件，是这个文件夹内所有东西的索引文件
然后随变vim一个wbtie中的文件，命令输入
set tags=/var/www/wbtie/tags
Tlist
这时候当光标停留在某一个函数或者变量那里的时候左侧也会动态的高亮显示，当你想知道这个变量或者函数是哪里定义的时候，只要敲下Ctrl+]就自动跳到它生命的地方了。 

```
### mac 注意
```
在配置自己的mac的开发环境时，一直没有把vim和ctags 解决好。导致打开文件时经常提示一些莫名的错误，今日有空，捣鼓了一下。


两个问题：

1）ctags不乖了。

可能用mac 的人会发现系统已经帮你安装了一个ctags了。不过那个并不是我们常用的，请到这里download一个新版本的，然后按说明编译之。

http://sourceforge.net/projects/ctags/
完事后，你可能发现仍然不行，我们常用的ctags -R啥的命令都木有，报错大概是：

usage: ctags [-BFadtuwvx] [-f tagsfile] file ...

原因很简单，系统上有两个ctags了。（我安装时没有专门去设置新装的ctags的路径，它被放到了/usr/local/bin下面，而mac os自带的是在/usr/bin/下的)

所以：修改PATH吧。类似这样：

export PATH="/usr/local/bin:$PATH:/Users/kevin/tools"
如果你对于kevin/tools也感兴趣，我也告诉你这里放了一个小脚本，用于生成ｔａｇｓ的。

[plain] view plain copy
#!/bin/sh  
#Filename: omnictags  
  
  
/usr/local/bin/ctags --c++-kinds=+p  --fields=+iaS  --extra=+q "$@"  

一般生成tags文件我就用omnictags -R dir 这样子，减少敲代码和记忆ctags的参数的需求。

2）taglist不乖了。

当你在vim中使用了插件taglist时，可能会报这样的错误。

Taglist: Failed to generate tags for xxxfile
ctags: illegal option -- -^@usage: ctags [-BFadtuwvx] [-f tagsfile] file ...^@
原因类似的，因为taglist插件找错了ctags了，故需要对taglist插件加一个配置，把下面这行添加到你的.vimrc配置中即可。


let Tlist_Ctags_Cmd='/usr/local/bin/ctags'

当然，目录视你的新ctags安装目录为准。


```