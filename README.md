# # Running Assembly on OS X

I’ve been learning a little assembly with some other Hacker Schoolers. It’s fun, but I spent nearly an hour yesterday trying to find a modern online guide for running x86-64 assembly on OS X, and couldn’t really find anything that didn’t involve complicated uses of `gcc`. Frankly, without the ever-wise advice of [davidad](http://davidad.github.io/), I probably wouldn’t have figured it out, so here’s a quick guide to what I did for future reference.


# Update nasm

`nasm` is the command you’ll use to assemble assembly code. The default version on my computer was old and didn’t have the proper formats, so just I used [homebrew](http://brew.sh/) to update it by installing a newer version.

```
$ brew install nasm
```

After I closed and reopened my terminal, I got the new version of  `nasm`, which had the critical  `macho64`  format.

```
$ nasm -v
NASM version 2.11 compiled on Mar  4 2014
$ nasm -hf
...
    macho32   NeXTstep/OpenStep/Rhapsody/Darwin/MacOS X (i386) object files
    macho64   NeXTstep/OpenStep/Rhapsody/Darwin/MacOS X (x86_64) object files
    dbg       Trace of all info passed to output stage
...

```

## An Example Assembly File

Stick this example assembly file in a  `hello_world.asm`  file in a new directory:

```
  %define SYSCALL_WRITE 0x2000004
  %define SYSCALL_EXIT  0x2000001

global start
start:
  mov rdi, 1
  mov rsi, str
  mov rdx, strlen
  mov rax, SYSCALL_WRITE
  syscall

  mov rax, SYSCALL_EXIT
  mov rdi, 0
  syscall

section .data
str:
  db `Hello, assembly!\n` ; to use escape sequences, use backticks
strlen equ $ - str
```

It should print  `hello, world`, and then exit.

## Using nasm and ld

To assemble your code, run:

```
# Generate object file from assembly:
nasm -f macho64 -o hello_world.o hello_world.asm

# Link object file:
ld hello_world.o -o hello_world

# Run executable:
./hello_world
```

If all goes well, you should see  `Hello, assembly!`  appear in your terminal. Good work!
