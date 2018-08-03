# Changelog

## 0.11.2 (03/08/2018)

- The module `Text::ASCIITable` is no longer needed. The text output is now markdown compatible.
- Some corrections raised by perlcritic. 

## 0.11.1 (31/07/2018)

- Added `./code2pg --help all_instructions`, in order to generate a markdown file containing all instructions. This could be piped to pandoc for example and give a nicely formatted html/pdf file. 

## 0.11.0 (16/07/2018)

- Adding comments in the html report. This was a feature request by EDF. 
- Bug correction in `--level[1..4]-minutes` command line switch.

## 0.10.3 (03/07/2018)

- Multiple extensions can now be analyzed at the same time.

## 0.10.2 (13/06/2018)

- Multiple directories can now be analyzed at the same time.
- Correction in some regexes.
- Default value for level4 changed to 20m. 

## 0.10.1 (30/05/2018)

- Preliminary work for other RDBMS.
- Added command line switch to choose the RDBMS and some variable renaming. This will allow DB2 / MySQL integration in the future.

## 0.10.0 (06/03/2018)

- Can be used with a configuration file (-c switch).
