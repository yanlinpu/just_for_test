## https://github.com/yanlinpu/use_vim_as_ide

### No.1 vim 源码下载安装
```
# 首先uninstall vim（如果已安装vim）
$ sudo apt-get remove vim
$ cd
# 下载源码
$ curl -O ftp://ftp.vim.org/pub/vim/unix/vim-7.4.tar.bz2
# 解压->vim74
$ tar jxvf vim-7.4.tar.bz2
$ cd vim74/  
$ ./configure --with-features=huge --enable-rubyinterp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config/ --enable-perlinterp --enable-gui=gtk2 --enable-cscope --prefix=/usr --enable-luainterp 
$ sudo make VIMRUNTIMEDIR=/usr/share/vim/vim74 && sudo make install
```
### No.2 插件管理
```
$ rm -rf ~/.vim
$ mkdir -p ~/.vim/bundle/pathogen/autoload/
$ cd ~/.vim/bundle/pathogen/autoload/
$ curl -LSso ./pathogen.vim https://tpo.pe/pathogen.vim
$ cd ~/.vim/bundle
# 主题插件可不要
$ git clone git://github.com/altercation/vim-colors-solarized.git 
# 美化状态栏
$ git clone git://github.com/Lokaltog/vim-powerline.git 
# 代码缩进
$ git clone git://github.com/nathanaelkane/vim-indent-guides.git 
# 接口与实现快速切换(没用到)
$ git clone git://github.com/vim-scripts/a.vim.git 
# 查看文件列表 ;bl 显示列表
$ git clone git://github.com/scrooloose/nerdtree.git 
# 多文档编辑  :b10 跳转到10的文件
$ git clone git://github.com/fholgado/minibufexpl.vim.git  
```
### No.3 编辑~/.vimrc
```
" 定义快捷键的前缀，即<Leader>                                                                                                                  
let mapleader=";"
" 开启文件类型侦测
filetype on
" 根据侦测到的不同类型加载对应的插件
filetype plugin on
" 将 pathogen 自身也置于独立目录中，需指定其路径 
runtime bundle/pathogen/autoload/pathogen.vim
" 运行 pathogen
execute pathogen#infect()
colorscheme default  
" set background=dark
" colorscheme solarized
" colorscheme molokai
" colorscheme phd
" 禁止光标闪烁
set gcr=a:block-blinkon0
" 总是显示状态栏
set laststatus=2
" 显示光标当前位置
set ruler
" 开启行号显示
set number
" 高亮显示当前行/列
set cursorline
set cursorcolumn
" 高亮显示搜索结果
set hlsearch
" 设置状态栏主题风格
let g:Powerline_colorscheme='solarized256'
" 开启语法高亮功能
syntax enable
" 允许用指定语法高亮配色方案替换默认方案
syntax on
" 自适应不同语言的智能缩进
filetype indent on
" 将制表符扩展为空格
set expandtab
" 设置编辑时制表符占用空格数
set tabstop=2
" 设置格式化时制表符占用空格数
set shiftwidth=2
" 让 vim 把连续数量的空格视为一个制表符
set softtabstop=2 
" 随 vim 自启动
let g:indent_guides_enable_on_vim_startup=1
" 从第二层开始可视化显示缩进
let g:indent_guides_start_level=2
" 色块宽度
let g:indent_guides_guide_size=1
" 快捷键 i 开/关缩进可视化
:nmap <silent> <Leader>i <Plug>IndentGuidesToggle
" 操作：za，打开或关闭当前折叠；zM，关闭所有折叠；zR，打开所有折叠。
" 基于缩进或语法进行代码折叠
set foldmethod=indent
" set foldmethod=syntax
" " 启动 vim 时关闭折叠代码
set nofoldenable
" 回车，打开选中文件；r，刷新工程目录文件列表；I（大写），显示/隐藏隐藏文件；m，出现创建/删除/剪切/拷贝操作列表。键入 <leader>fl 后，右边子窗口>为工程项目文件列表
" 使用 NERDTree 插件查看工程文件。设置快捷键，速记：file list
nmap <Leader>fl :NERDTreeToggle<CR>
" 设置NERDTree子窗口宽度
let NERDTreeWinSize=32
" " 设置NERDTree子窗口位置
let NERDTreeWinPos="right"
" 显示隐藏文件
let NERDTreeShowHidden=1
" NERDTree 子窗口中不显示冗余帮助信息
let NERDTreeMinimalUI=1
" 删除文件时自动删除文件对应 buffer
let NERDTreeAutoDeleteBuffer=1
"  ':bd', ':bw' or ':bun' ':b10'调到第10个窗口
" 显示/隐藏 MiniBufExplorer 窗口
map <Leader>bl :MBEToggle<cr>
" buffer 切换快捷键
map <C-Tab> :MBEbn<cr>
map <C-S-Tab> :MBEbp<cr>   
```
