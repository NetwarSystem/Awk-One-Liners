# Awk-One-Liners
Eric Pement's mighty Awk One Liners

I am not sure exactly why Eric Pement's HANDY ONE-LINE SCRIPTS FOR AWK went missing from pement.org, but it's a crisis on par with that time the little old lady in Minnesota with the recipe for ice cubes died. I rounded up a copy from Google cache and preserved it here, lest the entire Internet suddenly fail due to its lack.


HANDY ONE-LINE SCRIPTS FOR AWK                               30 April 2008
Compiled by Eric Pement - eric [at]pement.org               version 0.27
 
Latest version of this file (in English) isusually at:
  http://www.pement.org/awk/awk1line.txt
 
This file will also be available in otherlanguages:
  Chinese  - http://ximix.org/translation/awk1line_zh-CN.txt   
 
USAGE:
 
   Unix:awk '/pattern/ {print "$1"}'   # standard Unix shells
DOS/Win: awk '/pattern/ {print"$1"}'    # compiled withDJGPP, Cygwin
        awk "/pattern/ {print \"$1\"}"  # GnuWin32, UnxUtils, Mingw
 
Note that the DJGPP compilation (for DOS orWindows-32) permits an awk
script to follow Unix quoting syntax '/like/{"this"}'. HOWEVER, if the
command interpreter is CMD.EXE or COMMAND.COM,single quotes will not
protect the redirection arrows (<, >) nordo they protect pipes (|).
These are special symbols which require"double quotes" to protect them
from interpretation as operating systemdirectives. If the command
interpreter is bash, ksh or another Unix shell,then single and double
quotes will follow the standard Unix usage.
 
Users of MS-DOS or Microsoft Windows mustremember that the percent
sign (%) is used to indicate environmentvariables, so this symbol must
be doubled (%%) to yield a single percent signvisible to awk.
 
If a script will not need to be quoted in Unix,DOS, or CMD, then I
normally omit the quote marks. If an example ispeculiar to GNU awk,
the command 'gawk' will be used. Please notify meif you find errors or
new commands to add to this list (total lengthunder 65 characters). I
usually try to put the shortest script first. Toconserve space, I
normally use '1' instead of '{print}' to printeach line. Either one
will work.
 
FILE SPACING:
 
 # doublespace a file
 awk'1;{print ""}'
 awk'BEGIN{ORS="\n\n"};1'
 
 # doublespace a file which already has blank lines in it. Output file
 # shouldcontain no more than one blank line between lines of text.
 # NOTE: OnUnix systems, DOS lines which have only CRLF (\r\n) are
 # oftentreated as non-blank, and thus 'NF' alone will return TRUE.
 awk'NF{print $0 "\n"}'
 
 # triplespace a file
 awk'1;{print "\n"}'
 
NUMBERING AND CALCULATIONS:
 
 # precedeeach line by its line number FOR THAT FILE (left alignment).
 # Using atab (\t) instead of space will preserve margins.
 awk'{print FNR "\t" $0}' files*
 
 # precedeeach line by its line number FOR ALL FILES TOGETHER, with tab.
 awk'{print NR "\t" $0}' files*
 
 # numbereach line of a file (number on left, right-aligned)
 # Double thepercent signs if typing from the DOS command prompt.
 awk'{printf("%5d : %s\n", NR,$0)}'
 
 # numbereach line of file, but only print numbers if line is not blank
 # Remembercaveats about Unix treatment of \r (mentioned above)
 awk'NF{$0=++a " :" $0};1'
 awk'{print (NF? ++a " :" :"") $0}'
 
 # countlines (emulates "wc -l")
 awk'END{print NR}'
 
 # printthe sums of the fields of every line
 awk '{s=0;for (i=1; i<=NF; i++) s=s+$i; print s}'
 
 # add allfields in all lines and print the sum
 awk '{for(i=1; i<=NF; i++) s=s+$i}; END{print s}'
 
 # printevery line after replacing each field with its absolute value
 awk '{for(i=1; i<=NF; i++) if ($i < 0) $i = -$i; print }'
 awk '{for(i=1; i<=NF; i++) $i = ($i < 0) ? -$i : $i; print }'
 
 # printthe total number of fields ("words") in all lines
 awk '{total = total + NF }; END {print total}' file
 
 # printthe total number of lines that contain "Beth"
 awk'/Beth/{n++}; END {print n+0}' file
 
 # printthe largest first field and the line that contains it
 # Intendedfor finding the longest string in field #1
 awk '$1> max {max=$1; maxline=$0}; END{ print max, maxline}'
 
 # printthe number of fields in each line, followed by the line
 awk '{print NF ":" $0 } '
 
 # printthe last field of each line
 awk '{print $NF }'
 
 # printthe last field of the last line
 awk '{field = $NF }; END{ print field }'
 
 # printevery line with more than 4 fields
 awk 'NF> 4'
 
 # printevery line where the value of the last field is > 4
 awk '$NF> 4'
 
STRING CREATION:
 
 # create astring of a specific length (e.g., generate 513 spaces)
 awk'BEGIN{while (a++<513) s=s " "; print s}'
 
 # insert astring of specific length at a certain character position
 # Example:insert 49 spaces after column #6 of each input line.
 gawk--re-interval 'BEGIN{while(a++<49)s=s ""};{sub(/^.{6}/,"&" s)};1'
 
ARRAY CREATION:
 
 # Thesenext 2 entries are not one-line scripts, but the technique
 # is sohandy that it merits inclusion here.
 
 # createan array named "month", indexed by numbers, so that month[1]
 # is'Jan', month[2] is 'Feb', month[3] is 'Mar' and so on.
 split("Jan Feb Mar Apr May Jun Jul AugSep Oct Nov Dec", month, " ")
 
 # createan array named "mdigit", indexed by strings, so that
 #mdigit["Jan"] is 1, mdigit["Feb"] is 2, etc. Requires"month" array
 for (i=1;i<=12; i++) mdigit[month[i]] = i
 
TEXT CONVERSION AND SUBSTITUTION:
 
 # IN UNIXENVIRONMENT: convert DOS newlines (CR/LF) to Unix format
 awk'{sub(/\r$/,"")};1'   # assumesEACH line ends with Ctrl-M
 
 # IN UNIXENVIRONMENT: convert Unix newlines (LF) to DOS format
 awk'{sub(/$/,"\r")};1'
 
 # IN DOSENVIRONMENT: convert Unix newlines (LF) to DOS format
 awk 1
 
 # IN DOSENVIRONMENT: convert DOS newlines (CR/LF) to Unix format
 # Cannotbe done with DOS versions of awk, other than gawk:
 gawk -vBINMODE="w" '1' infile >outfile
 
 # Use"tr" instead.
 tr -d \r<infile >outfile            # GNUtr version 1.22 or higher
 
 # deleteleading whitespace (spaces, tabs) from front of each line
 # alignsall text flush left
 awk'{sub(/^[ \t]+/, "")};1'
 
 # deletetrailing whitespace (spaces, tabs) from end of each line
 awk'{sub(/[ \t]+$/, "")};1'
 
 # deleteBOTH leading and trailing whitespace from each line
 awk'{gsub(/^[ \t]+|[ \t]+$/,"")};1'
 awk'{$1=$1};1'           # also removesextra space between fields
 
 # insert 5blank spaces at beginning of each line (make page offset)
 awk'{sub(/^/, "     ")};1'
 
 # alignall text flush right on a 79-column width
 awk'{printf "%79s\n", $0}' file*
 
 # centerall text on a 79-character width
 awk'{l=length();s=int((79-l)/2); printf "%"(s+l)"s\n",$0}'file*
 
 #substitute (find and replace) "foo" with "bar" on each line
 awk'{sub(/foo/,"bar")}; 1'          # replace only 1st instance
 gawk'{$0=gensub(/foo/,"bar",4)}; 1' # replace only 4th instance
 awk'{gsub(/foo/,"bar")}; 1'         # replace ALL instances in a line
 
 #substitute "foo" with "bar" ONLY for lines which contain"baz"
 awk'/baz/{gsub(/foo/, "bar")}; 1'
 
 #substitute "foo" with "bar" EXCEPT for lines which contain"baz"
 awk'!/baz/{gsub(/foo/, "bar")}; 1'
 
 # change"scarlet" or "ruby" or "puce" to "red"
 awk'{gsub(/scarlet|ruby|puce/, "red")}; 1'
 
 # reverseorder of lines (emulates "tac")
 awk'{a[i++]=$0} END {for (j=i-1; j>=0;) print a[j--] }' file*
 
 # if aline ends with a backslash, append the next line to it (fails if
 # thereare multiple lines ending with backslash...)
 awk '/\\$/{sub(/\\$/,""); getline t; print $0 t; next}; 1' file*
 
 # printand sort the login names of all users
 awk -F":" '{print $1 | "sort" }' /etc/passwd
 
 # printthe first 2 fields, in opposite order, of every line
 awk'{print $2, $1}' file
 
 # switchthe first 2 fields of every line
 awk '{temp= $1; $1 = $2; $2 = temp}' file
 
 # printevery line, deleting the second field of that line
 awk '{ $2= ""; print }'
 
 # print inreverse order the fields of every line
 awk '{for(i=NF; i>0; i--) printf("%s ",$i);print ""}' file
 
 #concatenate every 5 lines of input, using a comma separator
 # betweenfields
 awk 'ORS=NR%5?",":"\n"'file
 
SELECTIVE PRINTING OF CERTAIN LINES:
 
 # printfirst 10 lines of file (emulates behavior of "head")
 awk 'NR< 11'
 
 # printfirst line of file (emulates "head -1")
 awk'NR>1{exit};1'
 
  # printthe last 2 lines of a file (emulates "tail -2")
 awk '{y=x"\n" $0; x=$0};END{print y}'
 
 # printthe last line of a file (emulates "tail -1")
 awk'END{print}'
 
 # printonly lines which match regular expression (emulates "grep")
 awk'/regex/'
 
 # printonly lines which do NOT match regex (emulates "grep -v")
 awk'!/regex/'
 
 # printany line where field #5 is equal to "abc123"
 awk '$5 =="abc123"'
 
 # printonly those lines where field #5 is NOT equal to "abc123"
 # Thiswill also print lines which have less than 5 fields.
 awk '$5 !="abc123"'
 awk '!($5== "abc123")'
 
 # matchinga field against a regular expression
 awk'$7  ~ /^[a-f]/'    # print line if field #7 matches regex
 awk '$7 !~/^[a-f]/'    # print line if field #7does NOT match regex
 
 # printthe line immediately before a regex, but not the line
 #containing the regex
 awk'/regex/{print x};{x=$0}'
 awk'/regex/{print (NR==1 ? "match on line 1" : x)};{x=$0}'
 
 # printthe line immediately after a regex, but not the line
 #containing the regex
 awk'/regex/{getline;print}'
 
 # grep forAAA and BBB and CCC (in any order on the same line)
 awk '/AAA/&& /BBB/ && /CCC/'
 
 # grep forAAA and BBB and CCC (in that order)
 awk'/AAA.*BBB.*CCC/'
 
 # printonly lines of 65 characters or longer
 awk'length > 64'
 
 # printonly lines of less than 65 characters
 awk'length < 64'
 
 # printsection of file from regular expression to end of file
 awk'/regex/,0'
 awk'/regex/,EOF'
 
 # printsection of file based on line numbers (lines 8-12, inclusive)
 awk'NR==8,NR==12'
 
 # printline number 52
 awk'NR==52'
 awk'NR==52 {print;exit}'          # moreefficient on large files
 
 # printsection of file between two regular expressions (inclusive)
 awk'/Iowa/,/Montana/'             # casesensitive
 
SELECTIVE DELETION OF CERTAIN LINES:
 
 # deleteALL blank lines from a file (same as "grep '.' ")
 awk NF
 awk '/./'
 
 # removeduplicate, consecutive lines (emulates "uniq")
 awk 'a !~$0; {a=$0}'
 
 # removeduplicate, nonconsecutive lines
 awk'!a[$0]++'                     # mostconcise script
 awk '!($0in a){a[$0];print}'      # most efficientscript
 
CREDITS AND THANKS:
 
Special thanks to the late Peter S. Tillier(U.K.) for helping me with
the first release of this FAQ file, and to DanielJana, Yisu Dong, and
others for their suggestions and corrections.
 
For additional syntax instructions, including theway to apply editing
commands from a disk file instead of the commandline, consult:
 
  "sed& awk, 2nd Edition," by Dale Dougherty and Arnold Robbins
 (O'Reilly, 1997)
 
 "UNIX Text Processing," by Dale Dougherty and Tim O'Reilly(Hayden
  Books,1987)
 
 "GAWK: Effective awk Programming," 3d edition, by Arnold D.Robbins
 (O'Reilly, 2003) or at http://www.gnu.org/software/gawk/manual/
 
To fully exploit the power of awk, one mustunderstand "regular
expressions." For detailed discussion ofregular expressions, see
"Mastering Regular Expressions, 3dedition" by Jeffrey Friedl (O'Reilly,
2006).
 
The info and manual ("man") pages onUnix systems may be helpful (try
"man awk", "man nawk","man gawk", "man regexp", or the section on
regular expressions in "man ed").
 
USE OF '\t' IN awk SCRIPTS: For clarity indocumentation, I have used
'\t' to indicate a tab character (0x09) in thescripts.  All versions of
awk should recognize this abbreviation.
 
#---end of file---
