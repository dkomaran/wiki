Finding Files On The Command Line

One of the things I like about Linux is the command line. I have used nautilus, gnome-commander, konqueror, kommander, dolphin and thunar to manage files in Linux and these file managers are great for what they do. But there are times when one simply wants to find a file when working on the command line without having to open a GUI application.

From the find man page:

GNU find searches the directory tree rooted at each given file name by evaluating the given expression from left to right, according to the rules of precedence until the outcome is known at which point find moves on to the next file name.

Find empty directories:

`find /path -depth -type d -empty`

Find empty files:

`find /path -depth -type f -empty`

Find a file with a specific name:

`find /path -name name_of_file`

Find a files with specific extensions:

`find /path -name "*.given_extension"`

Find files with specific permissions which have a ".txt. file extension:

`find /path -name '*.txt' -perm 644`

Find files with some given permissions:

`find /path -perm -permision_bits`

Find files with a given name and any extension:

`find /path -name 'given_name.*'`

Find files modified in the latest blocks of 24 hours:

`find /path -mtime n`

Where n is:

- 0 for the last 24 hours
- 1 for the last 48 hours
- 2 for the last 72 hours

Find files that were accessed in the latest blocks of 24 hours:

`find -atime n`

 *Where n is:*

- *0 for the last 24 hours*
- *1 for the last 48 hours*
- *2 for the last 72 hours *

Find files according to owner:

`find /path -user root`

One can also pipe find commands to the xargs command to execute commands on files.

Find and delete files:

`find /path -name mytestfile | xargs rm`

See man find and man xargs for more information about these powerful commands.

Many new Linux users are intimidated by the command line and this feeling should be overcome from the onset because the command line can be faster and more powerful than most GUI applications.
