# Mac

#### lsof on mac

```text

~/Downloads lsof -h
lsof 4.91
 latest revision: ftp://lsof.itap.purdue.edu/pub/tools/unix/lsof/
 latest FAQ: ftp://lsof.itap.purdue.edu/pub/tools/unix/lsof/FAQ
 latest man page: ftp://lsof.itap.purdue.edu/pub/tools/unix/lsof/lsof_man
 usage: [-?abhlnNoOPRtUvVX] [+|-c c] [+|-d s] [+D D] [+|-f[cgG]]
 [-F [f]] [-g [s]] [-i [i]] [+|-L [l]] [+|-M] [-o [o]] [-p s]
 [+|-r [t]] [-s [p:s]] [-S [t]] [-T [t]] [-u s] [+|-w] [-x [fl]] [--] [names]
Defaults in parentheses; comma-separated set (s) items; dash-separated ranges.
  -?|-h list help          -a AND selections (OR)     -b avoid kernel blocks
  -c c  cmd c ^c /c/[bix]  +c w  COMMAND width (9)    +d s  dir s files
  -d s  select by FD set   +D D  dir D tree *SLOW?*   -i select IPv[46] files
  -l list UID numbers      -n no host names           -N select NFS files
  -o list file offset      -O no overhead *RISKY*     -P no port names
  -R list paRent PID       -s list file size          -t terse listing
  -T disable TCP/TPI info  -U select Unix socket      -v list version info
  -V verbose search        +|-w  Warnings (+)         -X file descriptor table only
  -- end option scan
  +f|-f  +filesystem or -file names     +|-f[cgG] Ct flaGs
  -F [f] select fields; -F? for help
  +|-L [l] list (+) suppress (-) link counts < l (0 = all; default = 0)
  +|-M   portMap registration (-)       -o o   o 0t offset digits (8)
  -p s   exclude(^)|select PIDs         -S [t] t second stat timeout (15)
  -T fqs TCP/TPI Fl,Q,St (s) info
  -g [s] exclude(^)|select and print process group IDs
  -i i   select by IPv[46] address: [46][proto][@host|addr][:svc_list|port_list]
  +|-r [t[m<fmt>]] repeat every t seconds (15);  + until no files, - forever.
       An optional suffix to t is m<fmt>; m must separate t from <fmt> and
      <fmt> is an strftime(3) format for the marker line.
  -s p:s  exclude(^)|select protocol (p = TCP|UDP) states by name(s).
  -u s   exclude(^)|select login|UID set s
  -x [fl] cross over +d|+D File systems or symbolic Links
  names  select named files or files on named file systems
Anyone can list all files; /dev warnings disabled; kernel ID check disabled.

```

#### 生成man page的PDF文档

打开OS X的终端，通过man命令可以直接查看该命令的使用手册。但有时我们会觉得在命令行查看不太方便，如果可以提供一个PDF文档就完美了。这很容易做到，在终端输入如下命令，即可在预览程序打开grep的使用手册，另存为你需要的文件名即可：

```text

man -t grep | open -f -a Preview

```

#### file on mac

```text

~/Downloads file --help
Usage: file [OPTION...] [FILE...]
Determine type of FILEs.

      --help                 display this help and exit
  -v, --version              output version information and exit
  -m, --magic-file LIST      use LIST as a colon-separated list of magic
                               number files
  -M LIST                    use LIST as a colon-separated list of magic
                               number files in place of default
 LIST                    use LIST as a colon-separated list of magic
                               number files in place of default
  -z, --uncompress           try to look inside compressed files
  -Z, --uncompress-noreport  only print the contents of compressed files
  -b, --brief                do not prepend filenames to output lines
  -c, --checking-printout    print the parsed form of the magic file, use in
                               conjunction with -m to debug a new magic file
                               before installing it
  -d                         use default magic file
                         use default magic file
  -e, --exclude TEST         exclude TEST from the list of test to be
                               performed for file. Valid tests are:
                               apptype, ascii, cdf, compress, csv, elf,
                               encoding, soft, tar, json, text,
                               tokens
      --exclude-quiet TEST         like exclude, but ignore unknown tests
  -f, --files-from FILE      read the filenames to be examined from FILE
  -F, --separator STRING     use string as separator instead of `:'
  -i                         do not further classify regular files
                         do not further classify regular files
  -I, --mime                 output MIME type strings (--mime-type and
                               --mime-encoding)
      --extension            output a slash-separated list of extensions
      --mime-type            output the MIME type
      --mime-encoding        output the MIME encoding
  -k, --keep-going           don't stop at the first match
  -l, --list                 list magic strength
  -L, --dereference          follow symlinks
  -h, --no-dereference       don't follow symlinks (default)
  -n, --no-buffer            do not buffer output
  -N, --no-pad               do not pad output
  -0, --print0               terminate filenames with ASCII NUL
  -p, --preserve-date        preserve access times on files
  -P, --parameter            set file engine parameter limits
                                   bytes 1048576 max bytes to look inside file
                               elf_notes     256 max ELF notes processed
                               elf_phnum    2048 max ELF prog sections processed
                               elf_shnum   32768 max ELF sections processed
                                encoding   65536 max bytes to scan for encoding
                                   indir      50 recursion limit for indirection
                                    name      60 use limit for name/use magic
                                   regex    8192 length limit for REGEX searches
  -r, --raw                  don't translate unprintable chars to \ooo
  -s, --special-files        treat special (block/char devices) files as
                             ordinary ones
  -S, --no-sandbox           disable system call sandboxing
  -C, --compile              compile file specified by -m
  -D, --debug                print debugging messages

Report bugs to https://bugs.astron.com/

~/Downloads file green-leaves-1410259.jpg
green-leaves-1410259.jpg: JPEG image data, JFIF standard 1.02, resolution (DPI), density 72x72, segment length 16, Exif Standard: [TIFF image data, big-endian, direntries=11, manufacturer=FUJIFILM, model=FinePix A210  , orientation=upper-left, xresolution=170, yresolution=178, resolutionunit=2, software=Adobe Photoshop CS Windows, datetime=2005:01:23 17:55:48, copyright=    ], baseline, precision 8, 836x627, components 3

```

#### mdls on mac

mdls可以列出某个文件或文件夹的所有元数据信息，针对不同文件显示不同的元数据信息，例如文件创建时间、类型、大小等。如果是图片或音视频文件，则会显示更多元数据信息。

```text

~/Downloads mdls
(null): no filename specified!

usage: mdls [-name attr][-sdb] [-raw [-nullMarker markerString]] [-plist file] path
list the values of one or all the attributes of the specified file
  -raw:         don't print attribute names before values
  -nullMarker:  substitute this string for null attributes in raw mode
  -plist:       output attributes in XML format to file. Use - to write to stdout
                option -plist is incompatible with options -raw, -nullMarker, and -name
example:  mdls  ~/Pictures/Birthday.jpg
example:  mdls  -name Keyword ~/Pictures/Birthday.jpg

```

#### mdfind on mac

mdfind是一个非常灵活的全局搜索命令，类似Spotlight的命令行模式，可以在任何目录对文件名、文件内容进行检索。

```text

~/Downloads mdfind --help

Usage: mdfind [-live] [-count] [-onlyin directory] [-name fileName | -s smartFolderName | query]
list the files matching the query
query can be an expression or a sequence of words

	-attr <attr>      Fetches the value of the specified attribute
	-count            Query only reports matching items count
	-onlyin <dir>     Search only within given directory
	-live             Query should stay active
	-name <name>      Search on file name only
	-reprint          Reprint results on live update
	-s <name>         Show contents of smart folder <name>
	-0                Use NUL (``\0'') as a path separator, for use with xargs -0.

example:  mdfind image
example:  mdfind -onlyin ~ image
example:  mdfind -name stdlib.h
example:  mdfind "kMDItemAuthor == '*MyFavoriteAuthor*'"
example:  mdfind -live MyFavoriteAuthor

~ mdfind image | head
/System/Library/CoreServices/DiskImageMounter.app
/System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vImage.framework/Versions/A/Resources/BridgeSupport/vImage.bridgesupport
/System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vImage.framework/Versions/A/Resources/BridgeSupport/vImage.arm64e.bridgesupport
/System/Library/Frameworks/CoreImage.framework/Versions/A/Resources/BridgeSupport/CoreImage.bridgesupport
/System/Library/Frameworks/CoreImage.framework/Versions/A/Resources/BridgeSupport/CoreImage.arm64e.bridgesupport
/System/Library/Frameworks/Carbon.framework/Versions/A/Frameworks/ImageCapture.framework/Versions/A/Resources/BridgeSupport/ImageCapture.bridgesupport
/System/Library/Frameworks/Carbon.framework/Versions/A/Frameworks/ImageCapture.framework/Versions/A/Resources/BridgeSupport/ImageCapture.arm64e.bridgesupport
/System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/BridgeSupport/ImageIO.bridgesupport
/System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/BridgeSupport/ImageIO.arm64e.bridgesupport
/System/Library/Frameworks/ImageCaptureCore.framework/Versions/A/Resources/BridgeSupport/ImageCaptureCore.bridgesupport
~

```

#### open on mac

```shell
~/Downloads open -a
open: option requires an argument -- a
Usage: open [-e] [-t] [-f] [-W] [-R] [-n] [-g] [-h] [-s <partial SDK name>][-b <bundle identifier>] [-a <application>] [-u URL] [filenames] [--args arguments]
Help: Open opens files from a shell.
      By default, opens each file using the default application for that file.
      If the file is in the form of a URL, the file will be opened as a URL.
Options:
      -a                    Opens with the specified application.
      -b                    Opens with the specified application bundle identifier.
      -e                    Opens with TextEdit.
      -t                    Opens with default text editor.
      -f                    Reads input from standard input and opens with TextEdit.

# 打开 Chrome
open -a Google\ Chrome

# 使用默认文本编辑器打开一个文本
open -t some.txt

# 使用 Sublime 打开一个文本文件
open -a Sublime\ Text 88_confs.d_default.conf

# 使用 Xcode 打开一个文本文件
open -a Xcode some.txt
```

### [简单5步，解决macOS声音无法正常工作](https://www.sysgeek.cn/macos-fix-sound/)

打开「终端」——在终端中执行 `sudo killall coreaudiod` 命令

### M1 Mac 在某些 SSH 远端机器上无法换行

具体原因见 [stty returns 0 rows and columns on Apple M1](https://sourceforge.net/p/expect/bugs/106/), 卸载掉 brew install expect 5.45.4, 使用 Mac 系统自身的位于 `/usr/bin/expect` 的 expect, 确保 `expect -v` 返回 5.45 即可。

关于 expect 可参考 [Packages and Binaries: expect](https://www.kali.org/tools/expect/), [Expect/Expect](https://core.tcl-lang.org/expect/index).

### [在 Mac 上将 zsh 用作默认 Shell](https://support.apple.com/zh-cn/HT208050)

### [M1芯片Mac搭建前端开发环境](https://segmentfault.com/a/1190000039005726)
