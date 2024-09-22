## Bash (script) Test File Creator  ( *BTFcreate* )
 A test file generator that fills a file with random bytes.
 File contents can be
   Binary (default) from /dev/urandom.  (Not compressible)
   Printable (-P) any characters from the ASCI set.
   Digits only (-D)
   Sparse (-S) Linux, Unix and WSL2.
   The sparse files option is NOT to be used on MS Windows filesystems and WSL1, MSYS2, Gitbash and Cygwin - see below

 -f <file size>   Minimum 1 byte
    File sizes have to be designated by B, K, M or G
    Example 2KiB = 2K, 3MiB = 3M 4GiB = 4G

 Digits only
 -D n  Where n is a number 1 to 10.
   This selects the number of numerical characters required.
     n=1 file will be filled with the character zero '0'
     n=2 file will be filled with 0 and 1
     n=3 file will be filled with 0,1 and 2
     n=7 file will be filled with 0 and numbers 1 to 6

  All other printable ASCI characters.
  -P n Where n is a number 1 to 95.(the ASCI character set)
     n=1 file will filled with repeats of the character 'A'
     n>=2 or n<=26 all characters will be from the lower-case alphabet
     n>=27 characters will be from the 95 printable character set.
     Examples:
     n = 10 will select all characters stating at "a" to "j"
     n = 4 will select all characters "a" to "d"
     n = 60 will select all characters from "!" to "\\"
     n = 46 will select all characters from "!" to "M"

  -S Sparse file of nulls. Although disk usage is minimal their apparent size
     is used to determine usage in filesystem capacity and other areas.
     If used on a MS Windows volume the generated file will NOT be sparse.

VALIDATE CONTENTS
Validate all Files:
  od -N <bytes> -Ax -t x1z <file name>
     Where <bytes> is the number to check from beginning of file.
     Non-printable characters will be "dots"
     For sparse files omit <bytes> as it not required.

Validate printable character files - Distribution of printable characters:
  od -a <file name>  | cut -b 9- | tr " " \\n | egrep -v "^$" | sort | uniq -c

Validate sparse fILes verification:
  du -b <file name>      Shows disk usage in bytes.
  du -b --apparent-size  Shows the size the user sees and used filesystem capacity calculations
  ls -lsh                Shows disk usage at start of output, apparent size is in its usual place.


NOTES
Binary data that is stored/transmitted may render data deduplication ineffective.
The smaller range of printable characters used the longer the script runs.

WINDOWS SPARSE FILES - How to make:
On Windows run the following as administrator
  fsutil File CreateNew  <file name> <size in bytes>
  fsutil Sparse SetFlag  <file name>
  fsutil Sparse SetRange <file name> 0 <size in bytes>
WSL2-Linux cannot run windows program that require administrator privilege.
