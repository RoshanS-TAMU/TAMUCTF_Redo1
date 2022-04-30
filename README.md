# TAMUCTF_Redo1
Writeup for a Quick Reverse Engineering Challenge from TAMUCTF 2022
___________________________________________________________________
![image](https://media.github.tamu.edu/user/17583/files/bde59c80-c860-11ec-9ffb-c3890a49a2de)

Software reverse engineering, basic cryptography

The file seems to be a C file in human-readable code, so we will not need to decompile. 
```
d
```
My first idea was to compile and run the file:
```
d
```

No meaningful output was produced. I went back to the code file and analyzed it - the general purpose seems to be to take in user's guess and compare it to the flag - which is represented by a pointer to the character array A:
```
d
```

I ran the array through an online hex to text converter and and converted it from hex to text. The characters in the flag are readable, but do not conform with the standard flag format gigem{<flag>}. I suspected this is because of a mismatch in endians, so I converted the code from big-endian to little-endian, which produced a readable flag.
  ![image](https://media.github.tamu.edu/user/17583/files/f6d53f80-c867-11ec-8d4f-b207eaa9a4ad)
  ![image](https://media.github.tamu.edu/user/17583/files/a3172600-c868-11ec-9966-85fad3ec0b6a)
![image](https://media.github.tamu.edu/user/17583/files/b4603280-c868-11ec-8f0e-b0675b77efbf)

