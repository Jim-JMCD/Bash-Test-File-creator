## Bash (script) Test File Creator  ( *BTFcreate* )
 A test file generator that fills files with random bytes.  File contents can be
 
 *  Binary (default) from /dev/urandom.  (Not compressible)
 *  Printable (-P) any characters from the ASCI set. Optional
 *  Digits only (-D) Optioal
 *  Sparse (-S) only for Linux, Unix and WSL2. The sparse file option is __NOT__ to be used on MS Windows filesystems and WSL1, MSYS2, Gitbash and Cygwin - see below

### OPTIONS ###

__-f \<file size\>__   Manditory, minimum 1 byte. File sizes have to be designated by B, K, M or G. Example 2KiB = 2K, 3MiB = 3M 200GiB = 200G

__-o \<exisiting directory\>__  Optional, create file in a exisiting directory. Default is your current directory.  

#### File Contents ####

___Digits only:___ 

__-D n__     Where n is a number 1 to 10.
   
This selects the numbers in the character pool used
   
* n=1 files will be filled with the character zero '0'
* n=2 files will be filled with 0 and 1
* n=3 files will be filled with 0,1 and 2
* n=7 files will be filled with 0 and numbers 1 to 6
* n=10 files will be filled with a randon numbers of 0 to 9 

___All printable ASCI characters:___ 

__-P n__ Where n is a number 1 to 95.(the ASCI character set)
* n=1 file will filled with repeats of the character 'A'
* n>=2 or n<=26 all characters will be from the lower-case Latin alphabet
* n>=27 characters will be from the 95 printable character set.
  
_Examples:_
* n = 10 will select all characters stating at "a" to "j"
* n = 4 will select all characters "a" to "d"
* n = 60 will select all characters from "!" to "\\"
* n = :x46 will select all characters from "!" to "M"

__-S Sparse file of nulls__ Although disk usage is minimal their apparent size
     is used to determine usage in capacity usage.  Not to be used on Windows filesystems.

__VALIDATE FILE CONTENTS__

___Validate all Files:___

    od -N <bytes> -Ax -t x1z <file name>
     
* Where \<bytes\> is the number to check from beginning of file.
* Non-printable characters will be "dots"
* For sparse files omit <bytes> as it not required.

Validate printable character files - Distribution of printable characters:
   
    od -a <file name>  | cut -b 9- | tr " " \\n | egrep -v "^$" | sort | uniq -c

___Validate sparse fILes:___ 
  * du -b <file name>      Shows disk usage in bytes.
  * du -b --apparent-size  Shows the size the user sees and used filesystem capacity calculations
  * ls -lsh                Shows disk usage at start of output, apparent size is in its usual place.


__NOTES__
* Binary data may render storage and transmission data dedplication and compression ineffective.
* The smaller range of printable characters used the longer the script runs.
* Testing data duplication, the moost useful range is alphanumeric of 1 to 20 chracters i.e -P 1 to -P 20
* Testing compression, the most useful range is alphanumeric 1 to 40 i.e. -P 1 to -P 40

__WINDOWS SPARSE FILES - How to make:__

On Windows run the following as administrator

    fsutil File CreateNew  <file name> <size in bytes>
    fsutil Sparse SetFlag  <file name>
    fsutil Sparse SetRange <file name> 0 <size in bytes>


WSL2-Linux cannot run windows program that require administrator privilege.

_EXAMPLE_

   __BTFcreate -D 5 -f 5G -n 50__ 
   Create fifty 5GiB files containing only the randonly distrubeted digits 0, 1 , 2, 3 and 4   
   
   __BTFcreate -P 35 -f 1M -n 1__ 
   Create a single 1MiB files containing these characters randomly distributed __!"#$%&'()*+,-./0123456789:;<=>?\@ABC__
   
   __BTFcreate -P 35 -f 1M -n 1 -o ~/test_files__ 
   Same as previous, but with files being generated in the _test_files_ directory within the user's home directory. 
   
   
