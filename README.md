[![version.badge]](http://semver.org) [![travis.badge]][travis.url] [![appveyor.badge]][appveyor.url]

# Colout -- Add colors to a text stream in your terminal

Inspired by http://nojhan.github.com/colout/

This repo contents 2 versions of colout:

- A bash script (`colout`) and themes (`colout_*`).
- A more flexible but unfinished C++ version.

The following is only about the bash script.

## Synopsis

`colout` -h

`colout` -t [theme]

`colout` -l [COLORS_AND_STYLES...]

`colout` [-rcankRCeNpo] [-s scale] [-S group_index] [-i group_indexes] [-I group_index] [-u units] [-x awk_expr] [-X awk_expr] [-f awk_func] PATTERN [COLORS_AND_STYLES...] [-- ...]


## Installation

```bash
cp colout colout_* ~/bin
```


## Dependencies

- `gawk`
- `bash`


## Options

- `-t [theme]`:  List or apply. (load `${COLOUT_PREFIX_PATH:-${0}_}${theme}`)
- `-l`:  (line) Colors each lines.
- `-r`:  (repeat/again) Apply PATTERN until there is no more matching.
- `-a`:  (all) Loop on all PATTERN until there is no more matching.
- `-c`:  (continue) Try PATTERN even if previous ones match.
- `-n`:  (next) Each match passes to the next color.
- `-k`:  (keep) Use the previous color map.
- `-R`:  (no-reset) Do not reset colors.
- `-C`:  (color-continue) The current color becomes the normal color.
- `-e`:  (esc-reset) Reset the normal color.
- `-N`:  (lc-numeric) Use the locale's decimal point character when parsing input data.
- `-p`:  (print) print awk/sed command.
- `-o`:  (optimize) See man awk `-o`/`--optimize`.
- `-s scale`:  'min,max' or 'max'. Apply colors linearly between min and max (0,100 by default).
- `-S group_index`:  Use this group to compute the interval value. Implies `-s`.
- `-i group_indexes`:  Ignore groups (space and comma-separated list). Implies `-s`.
- `-I group_index`:  Shortcut for `-S group_index -i group_index`. Implies `-s`.
- `-u units`:  Cuts the color range per unit. Implies `-s`.
- `-x awk_expr`:  An expression that returns the color index.
  - `v`: the extracted value
  - `n`: the number of color
  - `u`: (only if `-u`) the unit position (`1` is first unit, `0` is an unrecognized unit).
- `-X awk_expr`:  An expression that returns a numeric value between min and max (see `-s scale`).
  - `v`: the extracted value
  - `n`: the number of color
  - `u`: (only if `-u`) the unit position (`1` is first unit, `0` is an unrecognized unit).
- `-f awk_script`:  Awk user functions.
- `-v awk_script`:  Awk user variables.


## PATTERN

  Regular expression.


## COLORS_AND_STYLES

- A style name
- A color name
- A colormap name
- A number in [0…255]
- `e` followed by a number in [0…15]
- `#` followed by 3 or 6 hexadecimal values
- `bg`
- Or a comma-separated list (or `=`) of those values.

### Styles

- `normal`,`n`
- `bold`,`o`
- `d[im]`
- `i[talic]`
- `u[nderline]`
- `blink`,`l`
- `reverse`,`v`
- `h[idden]`

### Named colors

An uppercase character is considered to be bold.

- `default`,`none`
- `[blac]k`
- `w[hite]`
- `r[ed]`
- `g[reen]`
- `b[lue]`
- `y[ellow]`
- `m[agenta]`
- `c[yan]`
- `gray`,`a`
- `d[ark_]gray`,`da`
- `l[ight_]red`,`lr`
- `l[ight_]green`,`lg`
- `l[ight_]blue`,`lb`
- `l[ight_]yellow`,`ly`
- `l[ight_]magenta`,`lm`
- `l[ight_]cyan`,`lc`

### Colormap

(`r:` reverse order)

- `[r:]default`,`Default`
- `[r:]rainbow`
- `[r:]Rainbow`
- `[r:]rainbow2`
- `[r:]Rainbow2`
- `[r:]spectrum`
- `[r:]Spectrum`
- `[r:]set2`
- `[r:]tab10`
- `[r:]brewer`
- `[r:]excel`
- `[r:]reds`
- `[r:]Reds`
- `[r:]greens`
- `[r:]Greens`
- `[r:]blues`
- `[r:]Blues`
- `[r:]purples`
- `[r:]Purples`
- `[r:]darkpurples`
- `[r:]navy`
- `[r:]br`
- `[r:]br2`

### rgb888 color

Decimal triplet: `[0-9]+/[0-9]+/[0-9]+` (ex: `123/213/42`).

### rgb888 color

Hexadecimal triplet: `#[0-9a-fA-F]{6}` (ex: `#22fa44`).

### rgb444 color

Hexadecimal triplet: `#[0-9a-fA-F]{3}` (ex: `#ae3`).

### 256 colors

A number in [0…255]

### ANSI/Escape color

`e` followed by a number in [0…15] (ex: `e3`).

### background prefix

`bg=` followed by a color.


## Example:

- `echo abcdefgh | ./colout '(..)..(..)' red,bg=#8f10e5,italic bg=yellow,k`
- `ls -shS ~/Videos | ./colout -Nu KMG -x 'log(v*(1024**(u-1)))*2*n/MAX-n' -v 'MAX=log(15000000)' '[0-9]+\.?,?[0-9]*.' rainbow2`
- `ls -shS ~/Videos | ./colout -Ns 1000 -u KMG '[0-9]+\.?,?[0-9]*.' rainbow`
- `ls -l | ./colout '(.)(.)(.)(.)(.)(.)(.)(.)(.)(.)' B {r,g,y},{,i,o}`
- `echo $LS_COLORS | ./colout -r '([^=]+)=([^:]+:)'`
- `echo $PATH | ./colout -rn '([^:]+):'`
- `env | ./colout '^([^=]+)=([^:]+)$|^([^=]+)=' y g y -- -cr '^([^=]+)=([^:]+):?' -- -c '.*' b`
- `echo 'Progress [########################] 100%' | ./colout -rn '#' hidden,bg=Rainbow2`
- `ls -l | ./colout -t perm`
- `echo ' ab "abc\\tde\\"fg\\""hi' | colout -aC '"' r,o -- -cr '\\.' y,o -- -ec '"' r`


<!-- links -->
[version.badge]: https://badge.fury.io/gh/jonathanpoelen%2Fcolout.svg

[travis.url]: https://travis-ci.org/jonathanpoelen/colout
[travis.badge]: https://travis-ci.org/jonathanpoelen/colout.svg?branch=master

[appveyor.url]: https://ci.appveyor.com/project/jonathanpoelen/colout
[appveyor.badge]: https://ci.appveyor.com/api/projects/status/github/jonathanpoelen/colout
