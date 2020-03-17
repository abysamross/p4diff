&nbsp;&nbsp;&nbsp;&nbsp;           
# p4diff
#### A ```p4 diff``` and ```p4 diff2``` combo script

I didn’t know if we had a bash script/tool to see the ```p4 diff``` and ```p4 diff2``` output in ```vimdiff```.
I understand that ```p4 diff``` output can be viewed as a ```vimdiff``` if we set ```P4DIFF``` variable to ```vimdiff```.
But ```p4 diff``` cannot be used to view diff of files not in your client.

Hence I wrote a script, ```p4diff```, combining functionality of both ```p4 diff``` and ```p4 diff2``` and opens the diff in ```vimdiff```.
It supports the following usages(this does not cover all the usages but the main ones):

1.  `p4diff`

&nbsp;&nbsp;&nbsp;&nbsp;Works as normal `p4 diff`. (Shows the diff of files opened in `edit/add/…`  modes in your workspace)

2.  `p4diff <filepath>`

    a. If file at `<filepath>` is present in your workspace and client.
&nbsp;&nbsp;&nbsp;&nbsp;NOTE: `<filepath>` in this case can be relative or absolute.

&nbsp;&nbsp;&nbsp;&nbsp;Works as `p4 diff <filepath>#<latest_version_in_depot>`
&nbsp;&nbsp;&nbsp;&nbsp;If your file `<version in your workspace>` is same as `<latest version in depot>`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Just says `“file(s) up-to-date”`.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Quit.
&nbsp;&nbsp;&nbsp;&nbsp;else
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Shows the `vimdiff` of your file `<version in your workspace>` version of the file with the `<latest version in depot>`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;You can also edit in place your workspace version and save the changes to it.
&nbsp;&nbsp;&nbsp;&nbsp;Quit.

    b. If file at `<filepath>` is not present in your workspace as per your client.
&nbsp;&nbsp;&nbsp;&nbsp;NOTE: `<filepath>` in this case has to be your absolute depot path.

&nbsp;&nbsp;&nbsp;&nbsp;Works as `p4 diff2 <filepath>#<latest_version_in_depot> <filepath>#<latest_version_in_depot>`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Just says `“No diff! Same versions of the same file”`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Quit.

3.  p4diff <filepath>#<version>

    (a) If file at <filepath> is present in your workspace and client.
        NOTE: <filepath> in this case can be relative or absolute.

            Works as p4 diff <filepath>#<version>
            If <version> is same as <version in your workspace>
                Just says “file up-to-date”.
                Quit.
            else
                Shows the vimdiff of file <version in your workspace> with the file <version> specified.
                You can also edit in place the file <version in your workspace> and save the changes to it.
                Quit.

    (b) If file at <filepath> is not present in your workspace as per your client.
        NOTE: <filepath> in this case has to be your absolute depot path.

            Works as p4 diff2 <filepath>#<version> <filepath>#<latest_version_in_depot>
            If <version> is same as <latest_version_in_depot>
                Just says “No diff! Same versions of the same file”.
                Quit.
            else
                Shows the vimdiff of depot <version> of the file with the <latest_version_in_depot>
                Quit.   

4.  p4diff <filepath> <filepath>
    Same as 2.

5.  p4diff <filename>#<version> <filename>
    Same as 3.

6.  p4diff <filepath>#<version1> <filepath>#<version2>

    (a) If file at <filepath> is present in your workspace and client. 
        NOTE: <filepath> in this case can be relative or absolute.

        i.  If <version2> is same as <version in your workspace>
                Works as p4 diff <filepath>#<version1>  
                NOTE: (same as 3(a))
                If <version1> is same as <version in your workspace>
                    Just says “file up-to-date”.
                    Quit.
            else
                Shows the vimdiff of file <version in your workspace> with the <version1> specified.
                You can also edit in place the file <version in your workspace> and save the changes to it.
                Quit.

        ii. else
                Works as p4 diff2 <filepath>#<version1> <filepath>#<version2>
                If <version1> is same as <version2>
                    Just says “No diff! Same versions of the same file”.
                    Quit.
                else
                    Shows the vimdiff of  depot <version1> and depot <version2>
                    You cannot edit or save changes in place.
                    Quit.

    (b) If file at <filepath> is not present in your workspace as per your client.
        NOTE: <filepath> in this case has to be your absolute depot path.

                Works as p4 diff2 <filepath>#<version1> <filepath>#<version2>
                If <version1> is same as <version2> 
                    Just says “No diff! Same versions of the same file”
                    Quit.
                else
                    Shows the vimdiff of  depot <version1> and depot <version2>
                    Quit.

7.  p4diff <filepath1> <filepath2>

    (a) If file at <filepath1> and file at <filepath2> are present in your workspace and client. 
        NOTE: <filepath> in this case can be relative or absolute.

        Works as p4 diff2 <filepath1>#<latest_version_in_depot> <filepath2>#<latest_version_in_depot>
        Shows the vimdiff of  file1 <latest version in depot> and file2 <latest version in depot>
        You cannot edit or save changes in place.
            Quit.

    (b) If file at <filepath1> and file at <filepath2> are not present in your workspace as per your client.
        NOTE: <filepath> in this case has to be your absolute depot path.

        Works as p4 diff2 <filepath1>#<latest_version_in_depot> <filepath2>#<latest_version_in_depot>
        Shows the vimdiff of  depot file1 <latest version in depot> and depot file2 <latest version in depot>
        Quit.

8.  p4diff <filepath1>#<version1> <filepath2>
    Same as (7) but <version2> for <filepath2> will be <latest_version_in_depot>

9.  p4diff <filepath1>#<version1> <filepath2>#<version2>
    Same as (7) but with <version1> and <version2> instead of <latest_version_in_depot>

General assumptions and errors:

a.  If the first <filepath1> argument does not have an associated  <version1> the <latest_version_in_depot> is taken by default.
b.  You can have a mix of workspace and depot <filepaths>, for steps (4) to (9) above, as your first and second arguments but you cannot modify any file in place.
c.  If the <version> for any <filepath> is less than 0 or greater than <latest_version_in_depot> the script throws an error and terminates. 
