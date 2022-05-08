# Wave a flag [10 points]
#### Category: General Skills
#### Author: SYREAL

## Description
Can you invoke help flags for a tool or binary? `This program` has extraordinarily helpful information...

## Hints:
1. This program will only work in the webshell or another Linux computer.
2. To get the file accessible in your shell, enter the following in the Terminal prompt:<br>
`$ wget https://mercury.picoctf.net/static/cfea736820f329083dab9558c3932ada/warm`
3. Run this program by entering the following in the Terminal prompt: `$ ./warm`, but you'll first have to make it executable with `$ chmod +x warm`
4. `-h` and `--help` are the most common arguments to give to programs to get more information from them!
5. Not every program implements help features like `-h` and `--help`.

# Solution
As I am too lazy to run this programm in a VM (I'm on a M1 macOS machine) I simply ran the `strings` command on the program:
```bash
$ strings ./warm
[SNIP]
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
```
This prints us every readable string the program contains.
Note: We are not seeing any flag... and scrolling through the output is a pain in the ***. So we try to `grep` for it.
```bash
$ strings ./warm | grep -o "picoCTF{.*}"
picoCTF{b1scu1ts_4nd_gr4vy_30e77291}
```
And that was non-intentional solution I guess...

#### Flag: `picoCTF{b1scu1ts_4nd_gr4vy_30e77291}`
