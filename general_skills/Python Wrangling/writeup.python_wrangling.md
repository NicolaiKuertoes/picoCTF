# Python Wrangling [10 points]
#### Category: General Skills
#### Author: SYREAL

## Description
Python scripts are invoked kind of like programs in the Terminal... Can you run `this Python script` using `this password` to get the `flag`?

## Hints:
1. Get the Python script accessible in your shell by entering the following command in the Terminal prompt:<br>
`$ wget https://mercury.picoctf.net/static/b351a89e0bc6745b00716849105f87c6/ende.py`
2. `$ man python`

# Solution
Running a Python script is simple - just type `python` or `python3` (depending on the Python version you've installed) followed by the <python_file_name>.py file you want to run, right in your terminal.
```bash
$ python3 ./ende.py
Usage: ende.py (-e/-d) [file]
```
So we ran the script but the output is not the flag. So let's have a look at the Python script itself:
```python
import sys
import base64
from cryptography.fernet import Fernet

usage_msg = "Usage: "+ sys.argv[0] +" (-e/-d) [file]"
help_msg = usage_msg + "\n" +\
        "Examples:\n" +\
        "  To decrypt a file named 'pole.txt', do: " +\
        "'$ python "+ sys.argv[0] +" -d pole.txt'\n"

if len(sys.argv) < 2 or len(sys.argv) > 4:
    print(usage_msg)
    sys.exit(1)

if sys.argv[1] == "-e":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "rb") as f:
        data = f.read()
        data_c = c.encrypt(data)
        sys.stdout.write(data_c.decode())


elif sys.argv[1] == "-d":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "r") as f:
        data = f.read()
        data_c = c.decrypt(data.encode())
        sys.stdout.buffer.write(data_c)


elif sys.argv[1] == "-h" or sys.argv[1] == "--help":
    print(help_msg)
    sys.exit(1)


else:
    print("Unrecognized first argument: "+ sys.argv[1])
    print("Please use '-e', '-d', or '-h'.")
```
As we can see, we can get a help message using the `-h` argument. So let`s give that a try:
```bash
$ python3 ./ende.py -h
Usage: ende.py (-e/-d) [file]
Examples:
  To decrypt a file named 'pole.txt', do: '$ python ende.py -d pole.txt'
```
Given the above help message, we can try to decrypt the `flag.txt.en`:
```bash
$ python3 ./ende.py -d flag.txt.en
Please enter the password:
```
Now we need to enter the correct password to decrypt the flag. Luckily the `pw.txt` file contains the password.
So let's read the password using `cat` in our terminal:
```bash
$ cat ./pw.txt
67c6cc9667c6cc9667c6cc9667c6cc96
```
Copy this password to our clipboard so that we can paste it later.
Running the Python script again:
```bash
$ python3 ./ende.py -d flag.txt.en
Please enter the password:67c6cc9667c6cc9667c6cc9667c6cc96
picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}
```
Yeah! We got the flag. But let's make this easier using some "commandline magic".  
We can use pipes `|` to redirect the stdout to the stdin of another command... Let's give it a shot:
```bash
$ cat pw.txt | python3 ende.py -d flag.txt.en
Please enter the password:picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}
```
As we can see, we did not have to enter the password manually - it was entered via the pipe `|`. 
However, we are only integrestet in the flag itself and non of the other output. So we can simply `grep` for the flag:
```bash
$ cat pw.txt | python3 ende.py -d flag.txt.en | grep -o "picoCTF{.*}"
picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}
```
Which shows us only the flag itself. To understand the `grep` command, have a look at the `man grep` page and do some research on "regular expressions".

We can go one step further and automatically save the flag as `flag.txt` using a redirect `>`:
```bash
$ cat pw.txt | python3 ende.py -d flag.txt.en | grep -o "picoCTF{.*}" > flag.txt
```
Note, that this command gives us no output but writes the output to a file called flag.txt.

We can check this by simply cat'ing the file:
```bash
$ cat flag.txt
picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}
```
And that's it!

#### Flag: `picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}`
