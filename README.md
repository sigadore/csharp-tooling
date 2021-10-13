# csharp-tooling
Set of Code and Documentation for working with C# cross platform

## Source Text Files
In my conversion project (from PowerBuilder to C#), I have discovered that the output of the
SnapDevelop Tool (and likely others in the Windows Universe) generate UTF-16 little endian
files complete with the [BOM] starting character stream.
Historically, difftools from GNU appears to be resistant to embrase the Unicode encoding
standards, in praticularly UTF-16, which it treated as indecypherable binary and as such
the line-by-line changes are difficult use diff or many other *nix character stream tools
that expect only ASCII.  I do not intend to comment on what appears to be a holy war, however
will merely point out that ASCII vs EBCDIC was equally devicive in the 1970s causing a good
deal of tools moving text from one platform to the other to have to spin their own solutions.

Given that this will likely not be resolved in time for your next project, even though MS
has purchased GitHub which relies on Git which in turn leverages 'diff' for the human readable
changes between commits.  So, what are the work arounds.

1. Live with these in their "Binary" form and give up meaningful git diff output.
2. Adjust the configuration to have your git (not GitHub) apply on the fly translation to UTF-8
3. Transform the text files involved from UTF-16 to UTF-8 (this to preserve non-ascii characters which may be in strings or variable names)


### Live with text files treated as "Binary"
This is the default action if you create content on Windows, espcecially translate from a Windows Based Tool.  It requires no
effort and git / GitHub will hapily store each revision for you, but when you are doing a 3-way merge, there are cases where
things "can't be auto merged" and you have no way to express what a "manual merge" is.  So, if every one is working on their
own branches and no cross platforn / cross sharing of changes to the same branch, it seems possible to work in this environment.
However, just me working on a Mac and a Windows box connected via GitHub (using Snap Develop's Git plugin).  I ran into impossible
to merge situations (which were fortunately not on the main branch) that required me to manually reapply the changes to a new
branch and then switch to using that one.

Text editors (including vim) are able to understand the UTF-16 and allow editing to proceed (many of them call these DOS liekly
because of the Carrage Return / Linefeed line termination sequence.

### Adjust the Configuration on a non-Windows git be abke to see the differences in file revisions
This trick involves the use of the iconv utility to convert stdin from UTF-16 to stdout as UTF-8 and involves:
1. Global Git Configuration file to get OS dependent changes that enables the interjection of this tool when diff is being used
2. A Configuration to call out which file types this extension should be applied to.

**Insert Config File changes here**

### Translate the csharp source files from UTF-16 to UTF-8
Either on a Branch or the mainline after the UTF-16 version is safely tucked away in your Git repository, translate the .cs files and other files
that you want to treat as lines of text to UTF-8 and check them in replacing the UTF-16 versions.  You'll have changed these files from
opaque binaries to text that diff will be happy with on the *nix computers as well as on GitHub (which can display UTB-16, but thats about it).

On a *nix system (That would be Linux and Mac included), the program `find` can be used in conjunction with other programs to identify UTF-16 files
of a specific extension (in this case `*.cs`) and apply `iconv` to the file and then replace the original.  While one can start with a simple script
to perform these operations, `find` with *nix shell naturally allows for a flow of the stream of files to have conditional processes (filters)
and then actions applied to them -- poor man's functional programming.  So, that refinement (one liners) are presented here in stages

**Insert find lines**
