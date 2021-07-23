code2pg
=======

<img src="./doc/logo_code _2_pg.png" width="120">


What is code2pg ?
--------------

`code2pg` is a tool that help migrating application code that contains SQL queries from Oracle to PostgreSQL standard.

It can:
- estimate in person-days how difficult a migration will be for application code (in Java files for example);
- tag the source file for Oracle instructions that might need to be migrated;
- generate different reports: html, text or minimal;
- connect directly to a SVN repository or use local files;
- be tuned according to the team's expertise.

Prerequisites :
-------------

`code2pg` is a standalone script. It requires Perl >= 5.20 and the `File::Slurp`, `File::Find::Rule`, `List::MoreUtils`, `Getopt::Long` and `Config::General` Perl modules. They can be obtained either from a CPAN client or from your distribution.

On a Centos 7(+) box, it can be installed this way:

```
sudo yum install perl-File-Slurp perl-File-Find-Rule perl-Config-General perl-List-MoreUtils perl-Getopt-Long
```

On a Debian box, the packages could be installed with:

```
sudo apt-get install libfile-slurp-perl libfile-find-rule-perl libconfig-general-perl liblist-moreutils-perl libgetopt-mixed-perl
```

The script has been tested very lightly on Windows with Strawberry Perl (v5.24.3) with the proper modules installed (the CPAN client can be used for this). A warning is issued though as `wc` is usually not recognized.

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

Please refer to the complete [generated documentation](https://github.com/societe-generale/code2pg/blob/master/doc/code2pg.pod) for more details. Here are a few examples:

- show the command line options of the tool:
```
./code2pg --help
```
- get some help on a specific instruction. The output is in markdown format:
```
./code2pg --help COMMIT
```
- get some help on all available instructions and generate a pdf file :
```
./code2pg --help ALL_INSTRUCTIONS | pandoc -f markdown -o myfile.pdf
```
- analyze files in the current directory with an extension .java and generate an estimation.html report. SQL instructions will have to searched in comma delimited strings:
```
./code2pg -e java -l comma-strings
```
- analyze java, jsp and .properties files in the current directory. This will so far need two reports as Oracle instructions will need to be analyzed in comma delimited strings, but directly in .properties files:
```
./code2pg -e java -e jsp -l comma-strings -o myproject_java.html
./code2pg -e properties -l plain -o myproject_properties.html
```
- analyze plsql files in two directories with extension .properties and generate a named html report. When SQL files must be analyzed directly (such as pl/sql or .properties files), please configure the language as "plain". For other languages, Oracle instructions will be searched between string delimiters.
```
./code2pg -e properties -l plain -d /tmp/project1 -d /tmp/project2 -o project_estimate.html
```
- analyze java files in a SVN repository and generate a text report:
```
./code2pg -D svn -d https://mysvnrepo/project/trunk -l comma-strings -e java -f txt
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
Estimation: 0.19 person-days
```

- text output:

```
Settings
========

- Version code2pg: 0.13.0
- Analysis date: 29/04/2019
- Source code directory: /home/user/migration-project/
- Number of .java files: 76
- Language: comma-strings
- Number of analyzed LOC: 0
- Log file: 
- orafce usage: No

Estimates
=========

|            | Number of instructions | Time/instruction | Estimated time (minutes) | person-days |
|------------|------------------------|------------------|--------------------------|-------------|
| Level 1    |                    265 |                1 |                      265 |         0.7 |
| Level 2    |                     27 |                4 |                      108 |         0.3 |
| Level 3    |                     26 |                8 |                      208 |         0.6 |
| Level 4    |                    140 |               20 |                     2800 |         7.8 |
| estimation |                    458 |              7.4 |                     3381 |         9.4 |

Instructions
============

| Level | Instruction          | Number |
|-------|----------------------|--------|
|     1 |                ASCII |     60 |
|     1 |                COUNT |     11 |
|     1 |                LOWER |    178 |
|     1 |                  MAX |      4 |
|     1 |                  MIN |      9 |
|     1 |                UPPER |      3 |
|     2 |                  NVL |      8 |
|     2 |                  SUM |     17 |
|     2 |            TO_NUMBER |      2 |
|     3 |                 CAST |      1 |
|     3 |               DECODE |     20 |
|     3 |                HINTS |      4 |
|     3 |               ROWNUM |      1 |
|     4 |              CONVERT |    140 |
```

- html output: 

![code2pg html output screenshot](doc/code2pg_report_html.jpg)

- help output:
````
ADD_MONTHS
==========

ORACLE
------

- Instruction: ADD_MONTHS
- Documentation: http://docs.oracle.com/database/121/SQLRF/functions011.htm

POSTGRESQL
-----------

- Postgresql instruction: No equivalent
- Documentation: 
- Level 3 - estimated time : 8 minutes.

COMMENTS
---------

A function can be created. For example: 
```
CREATE OR REPLACE FUNCTION public.add_months(date date, months integer)
 RETURNS date
 LANGUAGE plpgsql
AS $function$
BEGIN
RETURN (date + (months * '1 month' :: interval)) :: date;
END;
$function$
```
````

How to contribute?
------------------

See [CONTRIBUTING.md](CONTRIBUTING.md)

License
--------
License is under the 2-Clause BSD License.  
[![License](https://img.shields.io/badge/License-BSD%202--Clause-orange.svg)](LICENSE.md)
