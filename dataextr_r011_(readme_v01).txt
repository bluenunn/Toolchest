
File: dataextr_r011_(readme_v01).txt                    {Modified: 05/05/20, Created: 11/22/19, Author: Bill Nunn}

Overview:

     DataExtr utility is used to run queries on raw character delimited data sources (ex: CSV files) based upon a user defined “scope” (ordered subset of fields
     present in the raw data source "schema").  Queries may be refined using a set of optional parms ("query=", “include=”, “exclude=”, “sort=”) applied in the
     order listed.  These parms may be set in "config=<cfg_file>" or on the CLI.  "query=/sort=" parms are "field-specific" meaning they reference field labels
     defined using "schema=|scope=" parms ("sort=" may only reference "scope=" parm fields).  "include=/exclude=" parms are "field-generic" meaning the regular
     expression set in "[in|ex]clude=" applies to entire row of data ("target=schema") or to user defined subset of fields for given data row ("target=scope").
     Most queries are achievable using "query=/sort=" parms ("field-specific").  "[in|ex]clude=" parms ("field-generic") are legacy but may be used if desired. 
     Currently, DataExtr supports 3 delimiters (“,” “:” “;”) but could be expanded to include any printable character not present as data in the raw input file.
     DataExtr’s primary use case is to be able to quickly & easily provide multiple customized reports from the same raw data source ("rows=<rows>") to address
     different organizational needs without having to set up and maintain a traditional database environment.

Demos:

     DataExtr utility demos available upon request.  Email bluenunn@gmail.com to arrange.

Repository: https://github.com/bluenunn/Toolchest/

     - dataextr_r011_(docs_v01).txt         {DataExtr Users Guide: "Option/Parm" definitions, RegEx support}

     - dataextr_r011_(readme_v01).txt       {DataExtr Readme File: sample Syntax, Config, Data, Runs & History}

Sample Syntax:

     # Sample "lexical" sort syntax (<alpha>: "first,last"):         ("first,last": "lexi" sort performed on fields with <non-digit> characters)---::::::::::
                                                                                                                                                   ::::::::::
          ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"         # {Run01 ("first,last")    lexical forward sort}
                                                                                                                                                   ::::::::::
          ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"      # {Run02 ("first,last -r") lexical reverse sort}
                                                                                                                                                              ::
     # Sample "numeric" sort syntax (<integer>: "id_#"):                    ("-r": "reverse" sort flag, sort defaults to "forward" if "-r" flag is omitted)---::

          ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n"            # {Run03 ("id_# -n")       numeric forward sort}
                                                                                                                                                   :::: ::
          ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n -r"         # {Run04 ("id_# -n -r")    numeric reverse sort}
                                                                                                                                                   :::: :: ::
                           ("id_# [-n] [-r]": Utility formats <integer> fields as "%d" ("decimal"), sorts are "lexical" if "-n" flag is omitted)---::::-::-::
                                                                                                                                                   :::: :: ::
     # Sample "numeric" sort syntax (<fraction>: "ht_(ft)"):                                          (sort="id_# [-n] [-r]": sample sort types)---::::-::-::

          ./dataextr -dfi config=config_luna06_csv query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england' sort="ht_(ft) -n"       # {Run05 ("ht_(ft) -n")    numeric forward sort}

          ./dataextr -dfi config=config_luna06_csv query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england' sort="ht_(ft) -n -r"    # {Run06 ("ht_(ft) -n -r") numeric reverse sort}

Sample Config File:

     # Sample "config=<config>" parm file ($config):

          LAPTOP-MOQUDB6E:/tmp $ cat config_luna06_csv
          title="Luna Mendez Fan Club"
          rows=rows_luna06_csv
          chart=chart_luna06
          schema="First,Middle,Last,ID_#,Nickname,Title,Dept,Region,Country,Continent,Birthday,Ht_(ft)"
          scope="ID_#,First,Last,Birthday,Ht_(ft)",Title,Nickname,Region,Country,Continent

Sample Data File:

     # Sample "rows=<rows>" raw data file ($rows):

          LAPTOP-MOQUDB6E:/tmp $ cat rows_luna06_csv
          Ariel,,,11,Doncella de Agua,Disney Princess,Maidens Guild,Watertown,Atlantic Ocean,Atlantis,1983_02-07,5.21
          Bugs,,Bunny,21,Coniglio di Carota,Elmer Nemesis,Looney Tunes,Carrot Patch,USA,North America,1962_08-19,1.3
          Daffy,,Duck,20,Anatra Allegra,Zany Mallard,Looney Tunes,Carrot Patch,USA,North America,1965_04-25,1.27
          Edgar,,Uribe,7,Programador Maestro,IT Warrior,Programmers Guild,Morgan Hill,USA,North America,1995_12-03,5.91
          Elmer,,Fudd,14,Cacciatore di Conigli,Bugs Nemesis,Looney Tunes,Carrot Patch,USA,North America,1958_05-16,5.01
          Jasmine,,,17,Fanciulla di Tappeto Volante,Disney Princess,Maidens Guild,Desert Sands,Middle East,Asia,1987_09-26,4.111
          Jiminy,,Cricket,18,Insetto che Rimbalza,Disney,Geppetto Family,Tuscony,Italy,Europe,1968_01-14,0.092
          Juan,Jesus,Sierra,3,Scriba Reale,Intern,Round Table,Fremont,USA,North America,2002_10-25,5.82
          Max,,Mendez,2,Muscoli a Bizzeffe,Knight,Round Table,Sicily,Italy,Europe,1993_06-30,5.6
          Mike,,Snell,4,Gufo Saggio,,Wizards Guild,Elk Grove,USA,North America,1956_03-28,5.921
          Mulan,,,9,Principessa guerriera,Disney Princess,Royal Court,Beijing,China,Asia,2002_03-13,5.5
          Pinocchio,,,12,Ragazzo di Legno,Nosy,Geppetto Family,Lazio,Italy,Europe,1961_07-07,2.301
          Pocahantas,,,15,Fanciulla Indiana,Disney Princess,Maidens Guild,Enchanted Forest,USA,North America,1999_07-07,05.730
          Porky,,Pig,8,Maiale Porcile,Honored Luau Guest,Looney Tunes,Carrot Patch,USA,North America,1962_07-01,2.04
          Road,,Runner,10,Uccello Veloce,Coyote Nemesis,Looney Tunes,La Paz,Mexico,North America,1976_07-04,2.87
          Luna,,Mendez,1,Zampe Bianche,Princess,Royal Court,Calabria,Italy,Europe,2019_05-31,0.512
          Snow,,White,13,Mela del mio occhio,Disney Princess,Maidens Guild,Enchanted Forest,England,Europe,1952_02-29,5.6
          Speedy,,Gonzales,19,Mouse Molto Veloce,Cats Meow,Looney Tunes,Guadalajara,Mexico,North America,1967_07-12,0.48
          Stacy,,Yem,6,Maestro di Ceramica,IT Warrior,Artisans Guild,San Jose,USA,North America,1961_11-21,5.73
          Wiley,,Coyote,16,Coyote Sventato,Roadrunner Nemesis,Looney Tunes,La Paz,Mexico,North America,1975_03-10,4.021
          William,Evan,Nunn,5,Pescatore a Mosca,Craftsmens Guild,Masses,Redwood City,USA,North America,1958_05-16,6.2

Sample Runs:

     Run01 (sort="first,last")        {<alpha>: lexical forward sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"

          DataExtr Audit Complete: 2020-05-06_10.52.30_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          15    Pocahantas  _TBD_     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America

     Run02 (sort="first,last -r")     {<alpha>: lexical reverse sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"

          DataExtr Audit Complete: 2020-05-06_10.53.46_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          15    Pocahantas  _TBD_     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America

     Run03 (sort="id_# -n")           {<integer>: numeric forward sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n"

          DataExtr Audit Complete: 2020-05-06_10.54.31_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          15    Pocahantas  _TBD_     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America

     Run04 (sort="id_# -n -r")        {<integer>: numeric reverse sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="id_# -n -r"

          DataExtr Audit Complete: 2020-05-06_10.55.25_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America
          15    Pocahantas  _TBD_     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe

     Run05 (sort="ht_(ft) -n")        {<fraction>: numeric forward sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england' sort="ht_(ft) -n"

          DataExtr Audit Complete: 2020-05-06_11.04.13_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last    Birthday    Ht_(ft)  Title            Nickname               Region            Country  Continent
          14    Elmer       Fudd    1958_05-16  5.01     Bugs_Nemesis     Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          9     Mulan       _TBD_   2002_03-13  5.5      Disney_Princess  Principessa_guerriera  Beijing           China    Asia
          13    Snow        White   1952_02-29  5.6      Disney_Princess  Mela_del_mio_occhio    Enchanted_Forest  England  Europe
          15    Pocahantas  _TBD_   1999_07-07  05.730   Disney_Princess  Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          6     Stacy       Yem     1961_11-21  5.73     IT_Warrior       Maestro_di_Ceramica    San_Jose          USA      North_America
          3     Juan        Sierra  2002_10-25  5.82     Intern           Scriba_Reale           Fremont           USA      North_America
          7     Edgar       Uribe   1995_12-03  5.91     IT_Warrior       Programador_Maestro    Morgan_Hill       USA      North_America
          4     Mike        Snell   1956_03-28  5.921    _TBD_            Gufo_Saggio            Elk_Grove         USA      North_America

     Run06 (sort="ht_(ft) -n -r")     {<fraction>: numeric reverse sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dfi config=config_luna06_csv query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england' sort="ht_(ft) -n -r"

          DataExtr Audit Complete: 2020-05-06_11.05.08_PDT.

          DataExtr Return Code: 0     {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last    Birthday    Ht_(ft)  Title            Nickname               Region            Country  Continent
          4     Mike        Snell   1956_03-28  5.921    _TBD_            Gufo_Saggio            Elk_Grove         USA      North_America
          7     Edgar       Uribe   1995_12-03  5.91     IT_Warrior       Programador_Maestro    Morgan_Hill       USA      North_America
          3     Juan        Sierra  2002_10-25  5.82     Intern           Scriba_Reale           Fremont           USA      North_America
          6     Stacy       Yem     1961_11-21  5.73     IT_Warrior       Maestro_di_Ceramica    San_Jose          USA      North_America
          15    Pocahantas  _TBD_   1999_07-07  05.730   Disney_Princess  Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          13    Snow        White   1952_02-29  5.6      Disney_Princess  Mela_del_mio_occhio    Enchanted_Forest  England  Europe
          9     Mulan       _TBD_   2002_03-13  5.5      Disney_Princess  Principessa_guerriera  Beijing           China    Asia
          14    Elmer       Fudd    1958_05-16  5.01     Bugs_Nemesis     Cacciatore_di_Conigli  Carrot_Patch      USA      North_America

Sample History File ($history):

     Run06 History File:        {<fraction>: numeric reverse sort}

          LAPTOP-MOQUDB6E:/tmp $ cat /tmp/dataextr_history

          DataExtr History File: /tmp/dataextr_history

          Audit Timestamp (start):  2020-05-06_11.05.05_PDT

          Command Line: ./dataextr -dfi config=config_luna06_csv query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england' sort="ht_(ft) -n -r"

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

                "config" (cli):  config=config_luna06_csv

                 "title" (cfg):  title="Luna Mendez Fan Club"

                 "delim" (def):  delim=,

               "history" (def):  history=dataextr_history

                  "rows" (cfg):  rows=rows_luna06_csv

                 "chart" (cfg):  chart=chart_luna06

                "schema" (cfg):  schema=First,Middle,Last,ID_#,Nickname,Title,Dept,Region,Country,Continent,Birthday,Ht_(ft)

                 "scope" (cfg):  scope=ID_#,First,Last,Birthday,Ht_(ft),Title,Nickname,Region,Country,Continent

                "target" (def):  target=scope

                 "query" (cli):  query='ht_(ft)>5 && ht_(ft)<=6 && country~usa|china|england'

               "include" (def):

               "exclude" (def):

                  "sort" (cli):  sort="ht_(ft) -n -r"

          Commands:

               RC: 0     Command:  sort -t , -k5 -n -r /tmp/datatmp_chart06_jus > /tmp/datatmp_chart07_srt

          DataExtr Stage Directory ($dir): /tmp

          DataExtr Input  File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output File   ($chart): /tmp/chart_luna06

          Audit Timestamp (finish): 2020-05-06_11.05.08_PDT
