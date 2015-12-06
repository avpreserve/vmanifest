# vmanifest
Bash script (mac os) to verify unconventional md5 manifest structure: when a directory contains individual .md5 files for each content file.

Installation:

    Make the script executable on your machine. Change to the directory where your script is. Then use:
    chmod 777 vmanifest.sh

Usage:

    $ vmanifest.sh /full/path/to/files   

If you don't put the script in your path, you'll need to run it by addressing its full path, for example:

    If your working directory is the directory where the script is:
        ./vmanifest.sh [/full/Path/Of/Content/And/Md5s]
        
    If your working directory is not the same directory as the script:
        /Users/bertramlyons/desktop/tools/vmanifest/vmanifest.sh [/full/Path/Of/Content/And/Md5s]

Current assumptions include:

1) All files must be in the same directory (with no subdirectories).

2) The script assumes that for each content file there is an ancillary .md5 file (raw text) that contains one line in the following structure:

    [md5] [file name]
    (these two variables are delimited by a single space)

3) The script will write the results to a new folder created on the user's desktop called "md5_verification".

4) That md5 is the algorithm used for the original checksums.


Functionality:

1) User submits a target path (must be verbose from the root, not relative).

2) Script changes directory to the target path.

3) Script reads through each .md5 file, extracts the [md5] [filename] line and writes the line to a new, accumulating manifest.txt file, until single complete manifest is achieved.

4) Script reads through each content file (all that do not have .md5 as their extension), generates an md5 hash and writes the new hash and filename (in same [md5] [filename] schema as the previously created manifest) to a new manifest (nmanifest.txt) file.

5) Script performs a diff against the two manifest files and captures the result in a new variable.

6) If the result is a total success (manifests are identical), then the script creates a report to add to the md5_verification folder on the desktop, and prints the results in the terminal window and exits.

7) Otherwise, the script performs three logical tests to determine the appropriate failure response:

    a) There are too many files in the folder (more than there are original checksums)
    b) There are too few files in the folder (less than there are original checksums)
    c) There are non-matching checksums

8) The script reports the appropriate combination of responses, generates a report to add to the md5_verification folder on the desktop, and prints the results in the terminal window and exits.

Dangers:

Existence of .DS_Store files in the target folder may cause the script to report a failure, unless there is a corresponding .md5 file for the particular .DS_Store file

Not heavily tested for possible errors. The script was written to solve a specific use case. It may need to be adjusted as the use cases begin to vary.



