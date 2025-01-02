# Grep, awk, sed

## Grep

GREP = Global Regular Expression Print

File example :&#x20;

```bash
cat linepattern.txt              
test
toto
hi toto
how is he
toto is nice
his sister is vatotona
Hell yes !

```

Simple grep a string will returns all the lines where the string is present

<pre class="language-bash"><code class="lang-bash">cat linepattern.txt | grep "toto"
<strong>toto
</strong><strong>hi toto
</strong>toto is nice
his sister is vatotona
</code></pre>

### Basic Options

* Using -n to identify line numbers

```bash
cat linepattern.txt | grep -n "toto"
2:toto
3:hi toto
5:toto is nice
6:his sister is vatotona
```

* Using -v print the negative result

```bash
cat linepattern.txt| grep -v "toto"
test
how is he
Hell yes !

```

* Using -c to suppress the mathcing of a line and only display the number of lines that match

```bash
cat linepattern.txt | grep -c "toto"
4
```

* Using -x to match if a line jut contain the exact word

```bash
cat linepattern.txt | grep -xn "toto"
2:toto
```

### Regular expressions

* Match the lines by the last character

```bash
grep "e$" linepattern.txt
how is he
toto is nice
```

* grep -E "egrep" add a wider range of regular expressions. For instance ? after a character indicates that this is optionnal&#x20;

```bash
grep -E "v?a?toto" linepattern.txt
toto
hi toto
toto is nice
his sister is vatotona
```

## AWK

Structure of awk command :&#x20;

```
BEGIN { …. initialization awk commands …}
{ …. awk commands for each line of the file…}
END { …. finalization awk commands …}
```

* BEGIN = instrcutions before processing file's content (variables initialization)
* main {} section = Processes each line of the file
* END{} = instructions after processing file's content

&#x20;Each line is a number of fields, default field separator is one or more space characters

For instance :&#x20;

```
this is a line of text
```

This line contains 6 fields :&#x20;

$1 = "this" , $2 = "is" , ...\
$0 = "this is a line of text"

```bash
ls -l
total 104
-rwxr-xr-x  1 yanisyanis  staff  33426 15 jui 10:43 a.out
-rw-r--r--  1 yanisyanis  staff     75 16 jui 13:00 linepattern.txt
-rw-r--r--  1 yanisyanis  staff    777 15 jui 10:46 script.c
-rwxrwxrwx  1 yanisyanis  staff     74 15 jui 21:43 tcp.sh
-rw-r--r--  1 yanisyanis  staff     61 12 jui 13:30 your_first_latex.tex

ls -l | awk 'BEGIN {sum=0} {sum=sum+$5} END {print sum}'
34413
```

BEGIN {sum=0} = Init of sum variable

{sum=sum+$5} = Processing "ls-l" result output and size bytes column because it is the 5th word

END {print sum} = Printing the sum of each file size in bytes

## Sed

sed signifie Stream EDitor

```
sed 's/[D|d]ebian/Ubuntu/' /etc/os-release > /home/yanis/os-release
```

L'option -i permet d'écrire les modficiations dans le fichier appelé

```
sed -i 's/[D|d]ebian/Ubuntu/' /home/yanis/os-release
```

Afficher une sous-partie d'un fichier

```
sed -n 2,6p /etc/passwd
```

