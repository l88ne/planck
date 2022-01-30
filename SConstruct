import os
import sys
import fnmatch

target = "i686-elf"

env_options = {
    "CC"        : f"{target}-gcc",
    "CXX"       : f"{target}-g++",
    "LD"        : f"{target}-gcc",
    "AR"        : f"{target}-ar",
    "STRIP"     : f"{target}-strip",
    "AS"        : f"nasm",
    "CCFLAGS"   : "-ffreestanding -O2 -Wall -Wextra -fno-exceptions -fno-rtti",
    "LINKFLAGS" : "-T linker.ld -O2 -nostdlib -lgcc",
    "ASFLAGS"   : "-felf32"
}

env = Environment(**env_options)
env.Append(ENV = {"PATH": os.environ["PATH"]})

Export("env")

env.SConscript("src/SConscript", variant_dir="obj", duplicate=0)
env.SConscript("src/bootstrap/SConscript", variant_dir="obj/bootstrap", duplicate=0)

Default(env.Program("bin/kernel.bin", Glob("obj/*.o")
    + Glob("obj/bootstrap/*.o")))

env.Command('bin/kernel.iso', [ "bin/kernel.bin" ], "cp bin/kernel.bin etc/isoroot/boot && grub2-mkrescue -o bin/kernel.iso etc/isoroot")

if "qemu" in COMMAND_LINE_TARGETS:
    os.system("qemu-system-i386 -cdrom bin/kernel.iso")
    sys.exit(0)
