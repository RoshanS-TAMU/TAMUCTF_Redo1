# TAMUCTF_Redo1
Writeup for a Quick Reverse Engineering Challenge from TAMUCTF 2022
___________________________________________________________________
![image](https://media.github.tamu.edu/user/17583/files/bde59c80-c860-11ec-9ffb-c3890a49a2de)

Software reverse engineering, basic cryptography

The file seems to be a C file in human-readable code, so we will not need to decompile. 
```
#include <stdio.h>
#include <string.h>

#define STR_LEN 34

#define POINTER char* flag = (char*)(&a);
#define EXIT printf("Sorry that's not the flag\n"); return 1;
#define SUCCESS printf("THAT'S THE FLAG!\n"); return 0;
#define PARAMS printf("Usage: ./code <flag>\n"); return 1;


int main(int argc, char** argv) 
{
    int a[] = {0x65676967,0x00000000,0x34427b6d,0x5f433153,0x616c5f43,0x00000000,0x4175476e,0x525f4567,0x00000000,0x78305f45,0x53414c47,0x00007d53};

    if(argc != 2){ PARAMS }
    if(strlen(argv[1]) != STR_LEN){ EXIT }

    POINTER

    for(int i = 0; i < STR_LEN; i++)
    {
        int idx = i;
        if(i >= 4 && i <= 15){ idx += 4; }
        if(i >= 16 && i <= 23){ idx += 8; }
        if(i > 23){ idx += 12; }

        if(argv[1][i] != flag[idx]){ EXIT }
    }

    SUCCESS
}
```
My first idea was to compile and run the file:
```
$ g++ code.c
$ ./a.out
Usage: ./code <flag>
$ ./code aaaaaaaaaa
bash: ./code: No such file or directory

```

No meaningful output was produced. I went back to the code file and analyzed it - the general purpose seems to be to take in user's guess and compare it to the flag - which is represented by a pointer to the character array A:
```
int a[] = {0x65676967,0x00000000,0x34427b6d,0x5f433153,0x616c5f43,0x00000000,0x4175476e,0x525f4567,0x00000000,0x78305f45,0x53414c47,0x00007d53};
```

I ran the array through an online hex to text converter and and converted it from hex to text. The characters in the flag are readable, but do not conform with the standard flag format gigem{<flag>}. I suspected this is because of a mismatch in endians, so I converted the code from big-endian to little-endian, which produced a readable flag.
  ![image](https://media.github.tamu.edu/user/17583/files/f6d53f80-c867-11ec-8d4f-b207eaa9a4ad)
  ![image](https://media.github.tamu.edu/user/17583/files/a3172600-c868-11ec-9966-85fad3ec0b6a)
![image](https://media.github.tamu.edu/user/17583/files/ed98a280-c868-11ec-9230-b3149f2e6f23)


