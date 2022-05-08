# Obedient Cat [5 points]
#### Category: General Skills
#### Author: SYREAL

## Description
This file has a flag in plain sight (aka "in-the-clear").
## Hints:
1. Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.
2. To get the file accessible in your shell, enter the following in the Terminal prompt:<br>
`$ wget https://mercury.picoctf.net/static/33996e32dce022205a6a36f69aba56f0/flag`
3. `$ man cat`

# Solution
Everytime I get a file, I first take a look at the file-type of that file using the `file` command:
```bash
$ file ./flag
flag: ASCII text
```
Great! ASCII text is actually readable. So let's have a look at what the file contains using the `cat` command in our terminal:
```bash
$ cat ./flag
picoCTF{s4n1ty_v3r1f13d_2aa22101}
```
That was easy!
#### Flag: `picoCTF{s4n1ty_v3r1f13d_2aa22101}`
