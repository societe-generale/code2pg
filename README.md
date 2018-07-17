code2pg
=======

<img src="./doc/logo_code _2_pg.png" width="120">


What is code2pg ?
--------------

`code2pg` is a tool that help migrating application code that contains SQL queries from Oracle to PostgreSQL standard.

It can:
- estimate in man-days how difficult a migration will be for application code (in Java files for example);
- tag the source file for Oracle instructions that might need to be migrated;
- generate different reports: html, text or minimal;
- connect directly to a SVN repository or use local files;
- be tuned according to the team's expertise.

Prerequisites :
-------------

`code2pg` is a standalone script. It requires the `File::Slurp`, `File::Find::Rule`, `Text::ASCIITable` and `Config::General` Perl modules. They can be obtained either from a CPAN client or from your distribution.

On a Centos 7(+) box, it can be installed this way:

```
sudo yum install perl-Text-ASCIITable perl-File-Slurp perl-File-Find-Rule perl-Config-General
```

On a Debian box, the packages could be installed with:

```
sudo apt-get install libtext-asciitable-perl libfile-slurp-perl libfile-find-rule-perl libconfig-general-perl
```

The script has been tested on Windows with Strawberry Perl (v5.24.3) with the proper modules installed (the CPAN client can be used for this). A warning is issued though as `wc` is usually not recognized.

Installation
------------

```
git clone https://github.com/societe-generale/code2pg
```

The script should then be made executable:

```
chmod +x code2pg
```

Usage
-----

Besides the complete [generated documentation](https://github.com/societe-generale/code2pg/blob/master/doc/code2pg.pod), here a few examples:

- show the command line options of the tool:
```
./code2pg --help
```
- give some help on a specific instruction:
```
./code2pg --help COMMIT
```
- analyze java files in the current directory with an extension .java and generate an estimation.html report:
```
./code2pg -e java -l java
```
- analyze java, jsp and .properties files in the current directory. This will so far need two reports as Oracle instructins will need to be analyzed in "" delimited strings, but directly in .properties files:
```
./code2pg -e java -e jsp -l java -f myproject_java.html
./code2pg -e properties -l plsql -f myproject_properties.html
```
- analyze plsql files in two directories with extension .properties and generate a named html report. When SQL files must be analyzed directly (such as pl/sql or .properties files), please configure the language as plsql. For other languages, Oracle instructions will be searched between string delimiters.
```
./code2pg -e properties -l plsql -d /tmp/project1 -d /tmp/project2 -o project_estimate.html
```
- analyze java files in a SVN repository and generate a text report:
```
./code2pg -D svn -d https://mysvnrepo/project/trunk -l java -e java -f txt
```
- use a configuration file
```
./code2pg -c code2pg.conf
```

Report examples
---------------

- minimal output:

```
Done !
Estimation: 0.19 man-days
```

- text output:

```
.----------------------------------------------------------------------------------------------------------------.
|                                            code2pg                                                             |
'----------------------------------------------------------------------------------------------------------------'

Version                                  0.9
Analysis date                            05/10/2017
Source code directory                    /home/user/migration-project/
Number of files with extension .properties 13
Language                                 plsql
Number of analyzed LOC                   1826
Log file                                 
orafce usage                             No

.---------------------------------------------------------------------------------------------------------------.
|                             | Number of instructions | Time/instruction | Estimated time (minutes) | Man-days |
+-----------------------------+------------------------+------------------+--------------------------+----------+
| Level 1                     |                     52 |                1 |                       52 |     0.14 |
| Level 2                     |                    106 |                4 |                      424 |     1.18 |
| Level 3                     |                    118 |                8 |                      944 |     2.62 |
| Level 4                     |                      2 |               16 |                       32 |     0.09 |
+-----------------------------+------------------------+------------------+--------------------------+----------+
| Application code estimation |                    278 |             5.22 |                     1452 |     4.03 |
'-----------------------------+------------------------+------------------+--------------------------+----------'

.--------------------------------------.
| Level | Instruction         | Number |
+-------+---------------------+--------+
|     1 | DELETE_WITHOUT_FROM |     17 |
|     1 | DUAL                |      6 |
|     1 | MAX                 |      6 |
|     1 | UPPER               |      2 |
|     1 | USER                |     21 |
+-------+---------------------+--------+
|     2 | COMMIT              |      1 |
|     2 | NVL                 |     28 |
|     2 | ROLLBACK            |      1 |
|     2 | SYSDATE             |      4 |
|     2 | TO_DATE             |     72 |
+-------+---------------------+--------+
|     3 | FIRST               |     26 |
|     3 | LAST                |      5 |
|     3 | TO_CHAR             |     87 |
+-------+---------------------+--------+
|     4 | UID                 |      2 |
'-------+---------------------+--------'
```

- html output: 

![code2pg html output screenshot](doc/code2pg_report_html.jpg)

- help output:
```
[user@localhost curdir]$ ./code2pg -h ADD_MONTHS

ORACLE
======
 - Instruction: ADD_MONTHS
 - Documentation: http://docs.oracle.com/database/121/SQLRF/functions011.htm

POSTGRESQL
==========
 - Postgresql instruction: No equivalent
 - Documentation: 
 - Level 3 - estimated time : 8 minutes.

COMMENTS
========
  A function can be created. For example: CREATE OR REPLACE FUNCTION public.add_months(date date, months integer)
 RETURNS date
 LANGUAGE plpgsql
AS $function$
BEGIN
RETURN (date + (months * '1 month' :: interval)) :: date;
END;
$function$
```

How to contribute?
------------------

See [CONTRIBUTING.md](CONTRIBUTING.md)

License
--------
License is under the 2-Clause BSD License.  
[![License](https://img.shields.io/badge/License-BSD%202--Clause-orange.svg)](LICENSE.md)
