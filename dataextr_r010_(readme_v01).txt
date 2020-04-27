
File: dataextr_r010_(readme_v01).txt                    {Modified: 04/27/20, Created: 11/22/19, Author: Bill Nunn}

Overview:

     DataExtr utility is used to run queries on raw character delimited data sources (ex: CSV files) based upon a user defined “scope” (ordered subset of fields
     present in the raw data source "schema").  Queries may be refined using a set of optional parms ("query=", “include=”, “exclude=”, “sort=”) applied in the
     order listed.  These parms may be set in "config=<cfg_file>" or on the CLI.  "query=/sort=" parms are "field-specific" meaning they reference field labels
     defined using "schema=|scope=" parms ("sort=" may only reference "scope=" parm fields).  "include=/exclude=" parms are "field-generic" meaning the regular
     expression set in "[in|ex]clude=" applies to entire row of data ("target=schema") or to user defined subset of fields for given data row ("target=scope").
     Most queries are achievable using "query=/sort=" parms ("field-specific").  "[in|ex]clude=" parms ("field-generic") are legacy but may be used if desired. 
     Currently, DataExtr supports 3 delimiters (“,” “:” “;”) but could be expanded to include any printable character not present as data in the raw input file.
     DataExtr’s primary use case is to be able to quickly & easily provide multiple customized reports from the same raw data source ("rows=<rows>") to address
     different organizational needs without having to set up a traditional database environment.

Demos:

     DataExtr utility demos available upon request.  Email bluenunn@gmail.com to arrange.

Repository:  https://github.com/bluenunn/Toolchest/

     - dataextr_r010_(docs_v01).txt         {DataExtr Users Guide: "Option/Parm" definitions, RegEx support}

     - dataextr_r010_(readme_v01).txt       {DataExtr Readme File: sample Syntax, Config, Data, Runs & History File}

Sample Syntax, Config, Data, Runs & History File:

     # Sample "lexicographic" sort syntax:                          ("first,last": "lexi" sort performed on fields with <non-digit> characters)---::::::::::
                                                                                                                                                  ::::::::::
          ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"        # {Run01 ("first,last")    lexi forward sort}
                                                                                                                                                  ::::::::::
          ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"     # {Run02 ("first,last -r") lexi reverse sort}
                                                                                                                                                             ::
     # Sample "numeric" sort syntax:                                       ("-r": "reverse" sort flag, sort defaults to "forward" if "-r" flag is omitted)---::

          ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_#"              # {Run03 ("id_#")       numeric forward sort}
                                                                                                                                                  ::::
          ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n"           # {Run04 ("id_# -n")    numeric forward sort}
                                                                                                                                                  :::: ::
          ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n -r"        # {Run05 ("id_# -n -r") numeric reverse sort}
                                                                                                                                                  :::: :: ::
                  ("id_# [-n] [-r]": Utility formats <digit-only> fields as "%d" ("decimal"), sorts are "numeric" even if "-n" flag is omitted)---::::-::-::
                                                                                                                                                  :::: :: ::
     # Sample "config=..." parm file ($config):                                                      (sort="id_# [-n] [-r]": sample sort types)---::::-::-::

          LAPTOP-MOQUDB6E:/tmp $ cat config_luna05_csv
          title="Luna Mendez Fan Club"
          rows=rows_luna05_csv
          chart=chart_luna05
          schema="First,Middle,Last,ID_#,Nickname,Title,Dept,Region,Country,Continent,Birth_yr"
          scope="ID_#,First,Last,Title,Dept,Region,Country,Continent,Birth_yr"

     # Sample "rows=..." raw data file ($rows):

          LAPTOP-MOQUDB6E:/tmp $ cat rows_luna05_csv
          Ariel,,,00011,Doncella de Agua,Disney Princess,Maidens Guild,Watertown,Atlantic Ocean,Atlantis,1983
          Bugs,,Bunny,00021,Coniglio di Carota,Elmer Nemesis,Looney Tunes,Carrot Patch,USA,North America,1962
          Daffy,,Duck,00020,Anatra Allegra,Zany Mallard,Looney Tunes,Carrot Patch,USA,North America,1965
          Edgar,,Uribe,00007,Programador Maestro,IT Warrior,Programmers Guild,Morgan Hill,USA,North America,1995
          Elmer,,Fudd,00014,Cacciatore di Conigli,Bugs Nemesis,Looney Tunes,Carrot Patch,USA,North America,1958
          Jasmine,,,00017,Fanciulla di Tappeto Volante,Disney Princess,Maidens Guild,Desert Sands,Middle East,Asia,1987
          Jiminy,,Cricket,00018,Insetto che Rimbalza,Disney,Geppetto Family,Tuscony,Italy,Europe,1968
          Juan,Jesus,Sierra,00003,Scriba Reale,Intern,Round Table,Fremont,USA,North America,2002
          Max,,Mendez,00002,Muscoli a Bizzeffe,Knight,Round Table,Sicily,Italy,Europe,1993
          Mike,,Snell,00004,Gufo Saggio,,Wizards Guild,Elk Grove,USA,North America,1956
          Mulan,,,00009,Principessa guerriera,Disney Princess,Royal Court,Beijing,China,Asia,1989
          Pinocchio,,,00012,Ragazzo di Legno,Nosy,Geppetto Family,Lazio,Italy,Europe,1961
          Pocahantas,,,00015,Fanciulla Indiana,Disney Princess,Maidens Guild,Enchanted Forest,USA,North America,1980
          Porky,,Pig,00008,Maiale Porcile,Honored Luau Guest,Looney Tunes,Carrot Patch,USA,North America,1962
          Road,,Runner,00010,Uccello Veloce,Coyote Nemesis,Looney Tunes,La Paz,Mexico,North America,1976
          Luna,,Mendez,00001,Zampe Bianche,Princess,Royal Court,Calabria,Italy,Europe,2019
          Snow,,White,00013,Mela del mio occhio,Disney Princess,Maidens Guild,Enchanted Forest,England,Europe,1954
          Speedy,,Gonzales,00019,Mouse Molto Veloce,Cats Meow,Looney Tunes,Guadalajara,Mexico,North America,1967
          Stacy,,Yem,00006,Maestro di Ceramica,IT Warrior,Artisans Guild,San Jose,USA,North America,1961
          Wiley,,Coyote,00016,Coyote Sventato,Roadrunner Nemesis,Looney Tunes,La Paz,Mexico,North America,1975
          William,Evan,Nunn,00005,Pescatore a Mosca,Craftsmens Guild,Masses,Redwood City,USA,North America,1958

     # Sample Runs:

          Run_01 ("sort=first,last"):          {"lexicographic forward sort": "lexi" (field has <non-digit> chars), "forward" (default if "-r" is omitted)}

               LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"

               DataExtr Audit Complete: 2020-04-27_13.18.47_PDT.

               DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

               DataExtr Stage Directory  ($dir): /tmp

               DataExtr History File ($history): /tmp/dataextr_history

               DataExtr Input   File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output  File   ($chart): /tmp/chart_luna05

               Luna Mendez Fan Club:

               ID_#   First       Last      Title               Dept           Region            Country  Continent      Birth_yr
               00021  Bugs        Bunny     Elmer_Nemesis       Looney_Tunes   Carrot_Patch      USA      North_America  1962
               00020  Daffy       Duck      Zany_Mallard        Looney_Tunes   Carrot_Patch      USA      North_America  1965
               00014  Elmer       Fudd      Bugs_Nemesis        Looney_Tunes   Carrot_Patch      USA      North_America  1958
               00001  Luna        Mendez    Princess            Royal_Court    Calabria          Italy    Europe         2019
               00002  Max         Mendez    Knight              Round_Table    Sicily            Italy    Europe         1993
               00015  Pocahantas  _TBD_     Disney_Princess     Maidens_Guild  Enchanted_Forest  USA      North_America  1980
               00010  Road        Runner    Coyote_Nemesis      Looney_Tunes   La_Paz            Mexico   North_America  1976
               00019  Speedy      Gonzales  Cats_Meow           Looney_Tunes   Guadalajara       Mexico   North_America  1967
               00016  Wiley       Coyote    Roadrunner_Nemesis  Looney_Tunes   La_Paz            Mexico   North_America  1975

          Run_02 ("sort=first,last -r"):          {"lexicographical reverse sort": "-r" (reverse) flag is set}

               LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"

               DataExtr Audit Complete: 2020-04-27_13.19.10_PDT.

               DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

               DataExtr Stage Directory  ($dir): /tmp

               DataExtr History File ($history): /tmp/dataextr_history

               DataExtr Input   File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output  File   ($chart): /tmp/chart_luna05

               Luna Mendez Fan Club:

               ID_#   First       Last      Title               Dept           Region            Country  Continent      Birth_yr
               00016  Wiley       Coyote    Roadrunner_Nemesis  Looney_Tunes   La_Paz            Mexico   North_America  1975
               00019  Speedy      Gonzales  Cats_Meow           Looney_Tunes   Guadalajara       Mexico   North_America  1967
               00010  Road        Runner    Coyote_Nemesis      Looney_Tunes   La_Paz            Mexico   North_America  1976
               00015  Pocahantas  _TBD_     Disney_Princess     Maidens_Guild  Enchanted_Forest  USA      North_America  1980
               00002  Max         Mendez    Knight              Round_Table    Sicily            Italy    Europe         1993
               00001  Luna        Mendez    Princess            Royal_Court    Calabria          Italy    Europe         2019
               00014  Elmer       Fudd      Bugs_Nemesis        Looney_Tunes   Carrot_Patch      USA      North_America  1958
               00020  Daffy       Duck      Zany_Mallard        Looney_Tunes   Carrot_Patch      USA      North_America  1965
               00021  Bugs        Bunny     Elmer_Nemesis       Looney_Tunes   Carrot_Patch      USA      North_America  1962

          Run_03 ("sort="id_#"):          {"numeric forward sort": utility sort type for <digit-only> (%d) fields}

               LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_#"

               DataExtr Audit Complete: 2020-04-27_13.19.25_PDT.

               DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

               DataExtr Stage Directory  ($dir): /tmp

               DataExtr History File ($history): /tmp/dataextr_history

               DataExtr Input   File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output  File   ($chart): /tmp/chart_luna05

               Luna Mendez Fan Club:

               ID_#   First       Last      Title               Dept           Region            Country  Continent      Birth_yr
               00001  Luna        Mendez    Princess            Royal_Court    Calabria          Italy    Europe         2019
               00002  Max         Mendez    Knight              Round_Table    Sicily            Italy    Europe         1993
               00010  Road        Runner    Coyote_Nemesis      Looney_Tunes   La_Paz            Mexico   North_America  1976
               00014  Elmer       Fudd      Bugs_Nemesis        Looney_Tunes   Carrot_Patch      USA      North_America  1958
               00015  Pocahantas  _TBD_     Disney_Princess     Maidens_Guild  Enchanted_Forest  USA      North_America  1980
               00016  Wiley       Coyote    Roadrunner_Nemesis  Looney_Tunes   La_Paz            Mexico   North_America  1975
               00019  Speedy      Gonzales  Cats_Meow           Looney_Tunes   Guadalajara       Mexico   North_America  1967
               00020  Daffy       Duck      Zany_Mallard        Looney_Tunes   Carrot_Patch      USA      North_America  1965
               00021  Bugs        Bunny     Elmer_Nemesis       Looney_Tunes   Carrot_Patch      USA      North_America  1962

          Run_04 ("sort="id_# -n"):          {"numeric forward sort": "-n" (numeric) sort flag is set}

               LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n"

               DataExtr Audit Complete: 2020-04-27_13.19.41_PDT.

               DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

               DataExtr Stage Directory  ($dir): /tmp

               DataExtr History File ($history): /tmp/dataextr_history

               DataExtr Input   File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output  File   ($chart): /tmp/chart_luna05

               Luna Mendez Fan Club:

               ID_#   First       Last      Title               Dept           Region            Country  Continent      Birth_yr
               00001  Luna        Mendez    Princess            Royal_Court    Calabria          Italy    Europe         2019
               00002  Max         Mendez    Knight              Round_Table    Sicily            Italy    Europe         1993
               00010  Road        Runner    Coyote_Nemesis      Looney_Tunes   La_Paz            Mexico   North_America  1976
               00014  Elmer       Fudd      Bugs_Nemesis        Looney_Tunes   Carrot_Patch      USA      North_America  1958
               00015  Pocahantas  _TBD_     Disney_Princess     Maidens_Guild  Enchanted_Forest  USA      North_America  1980
               00016  Wiley       Coyote    Roadrunner_Nemesis  Looney_Tunes   La_Paz            Mexico   North_America  1975
               00019  Speedy      Gonzales  Cats_Meow           Looney_Tunes   Guadalajara       Mexico   North_America  1967
               00020  Daffy       Duck      Zany_Mallard        Looney_Tunes   Carrot_Patch      USA      North_America  1965
               00021  Bugs        Bunny     Elmer_Nemesis       Looney_Tunes   Carrot_Patch      USA      North_America  1962

          Run_05 ("sort="id_# -n -r"):          {"numeric reverse sort": "-n" (numeric) & "-r" (reverse) sort flags are set}

               LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n -r"

               DataExtr Audit Complete: 2020-04-27_13.20.03_PDT.

               DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

               DataExtr Stage Directory  ($dir): /tmp

               DataExtr History File ($history): /tmp/dataextr_history

               DataExtr Input   File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output  File   ($chart): /tmp/chart_luna05

               Luna Mendez Fan Club:

               ID_#   First       Last      Title               Dept           Region            Country  Continent      Birth_yr
               00021  Bugs        Bunny     Elmer_Nemesis       Looney_Tunes   Carrot_Patch      USA      North_America  1962
               00020  Daffy       Duck      Zany_Mallard        Looney_Tunes   Carrot_Patch      USA      North_America  1965
               00019  Speedy      Gonzales  Cats_Meow           Looney_Tunes   Guadalajara       Mexico   North_America  1967
               00016  Wiley       Coyote    Roadrunner_Nemesis  Looney_Tunes   La_Paz            Mexico   North_America  1975
               00015  Pocahantas  _TBD_     Disney_Princess     Maidens_Guild  Enchanted_Forest  USA      North_America  1980
               00014  Elmer       Fudd      Bugs_Nemesis        Looney_Tunes   Carrot_Patch      USA      North_America  1958
               00010  Road        Runner    Coyote_Nemesis      Looney_Tunes   La_Paz            Mexico   North_America  1976
               00002  Max         Mendez    Knight              Round_Table    Sicily            Italy    Europe         1993
               00001  Luna        Mendez    Princess            Royal_Court    Calabria          Italy    Europe         2019

     # Sample History File ($history):

          Run05 History File:          {"numeric reverse sort": "-n" (numeric) & "-r" (reverse) sort flags are set}

               LAPTOP-MOQUDB6E:/tmp $ cat dataextr_history

               DataExtr History File: /tmp/dataextr_history

               Audit Timestamp (start):  2020-04-27_13.20.01_PDT

               Command Line: ./dataextr -dfi config=config_luna05_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n -r"

               Data_Source: LAPTOP-MOQUDB6E          {User_ID: bluenunn, OS: Ubuntu 18.04.1 LTS}

               Return Code: 0        {"0": no errors found, "<non-zero>": errors detected (check History file)}

               Option Settings:      {"0": false, "1": true}     {Option Sources:  (def): default, (cli): CLI}

                    "-d" (cli):  1   {display}

                    "-f" (cli):  1   {fields}

                    "-i" (cli):  1   {ignore-case}

                    "-r" (def):  0   {raw}

                    "-t" (def):  0   {temporary}

               Parm Values:          {Parm Sources:  (def): default, (cfg): <cfg_file>, (cli): CLI, (sch): scope="$schema"}

                        "dir" (def):  dir=/tmp

                     "config" (cli):  config=config_luna05_csv

                      "title" (cfg):  title="Luna Mendez Fan Club"

                      "delim" (def):  delim=,

                    "history" (def):  history=dataextr_history

                       "rows" (cfg):  rows=rows_luna05_csv

                      "chart" (cfg):  chart=chart_luna05

                     "schema" (cfg):  schema=First,Middle,Last,ID_#,Nickname,Title,Dept,Region,Country,Continent,Birth_yr

                      "scope" (cfg):  scope=ID_#,First,Last,Title,Dept,Region,Country,Continent,Birth_yr

                     "target" (def):  target=scope

                      "query" (cli):  query='id_#>=10 && continent~north_America || last~mendez'

                    "include" (def):

                    "exclude" (def):

                       "sort" (cli):  sort="id_# -n -r"

               Commands:

                    RC: 0     Command:  sort -t , -k1 -n -r /tmp/datatmp_chart06_jus > /tmp/datatmp_chart07_srt

               DataExtr Stage Directory ($dir): /tmp

               DataExtr Input  File    ($rows): /tmp/rows_luna05_csv

               DataExtr Output File   ($chart): /tmp/chart_luna05

               Audit Timestamp (finish): 2020-04-27_13.20.03_PDT
