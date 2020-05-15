
File: dataextr_r012_(readme_v02).txt                    {Modified: 05/15/20, Created: 11/22/19, Author: Bill Nunn}

Overview:

     DataExtr utility is used to run queries on raw character delimited data sources (ex: CSV files) based upon a user defined “scope” (ordered subset of fields
     present in the raw data source "schema").  Queries may be refined using a set of optional parms ("query=", “include=”, “exclude=”, “sort=”) applied in the
     order listed.  These parms may be set in "config=<cfg_file>" or on the CLI.  "query=/sort=" parms are "field-specific" meaning they reference field labels
     defined using "schema=|scope=" parms ("sort=" may only reference "scope=" parm labels).  "include=/exclude=" parms are "field-generic" meaning the regular
     expression defined in "[in|ex]clude=" applies to entire row of data (target=schema) or to user defined subset of fields for given data row (target=scope).
     Most queries are achievable using "query=/sort=" parms ("field-specific").  "[in|ex]clude=" parms ("field-generic") are legacy but may be used if desired.
     DataExtr supports 8 field delimiters (", : ; @ # % & =").  Each data row must have the same # of fields.  Delimiters may not be part of any field's data.
     DataExtr’s primary use case is to be able to quickly & easily provide multiple customized reports from the same raw data source (rows=<rows>) to address
     different organizational needs without having to set up and maintain a traditional database environment.

Demos:

     DataExtr utility demos available upon request.  Email bluenunn@gmail.com to arrange.

Repository: https://github.com/bluenunn/Toolchest/

     - dataextr_r012_(docs_v01).txt                   {DataExtr Users Guide: "Option/Parm" definitions, RegEx support}

     - dataextr_r012_(readme_v01).txt                 {DataExtr Readme File: sample Syntax, Config, Data, Runs & History}

     - dataextr_r012_(rows_wine_equ_3k).txt"          {Sample Wine Industry Review data (3,022 rows), Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

Sample Syntax:

     # Sample "lexical" sort syntax (Organization Membership):     ("first,last": lexical sorts are typically used for non-<decimal> fields)---::::::::::
                                                                                                                                               ::::::::::
          ./dataextr -di config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"      # {Run01 ("first,last"):         lexical forward sort}
                                                                                                                                               ::::::::::
          ./dataextr -di config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"   # {Run02: first,last -r"):       lexical reverse sort}

     # Sample "numeric" sort syntax (Wine Industry Review): "<Points> vs <Price>" query):          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          ./dataextr -di  config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n"      # {Run03 ("Points,Price -n"):    numeric forward sort}

          ./dataextr -dfi config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n -r"   # {Run04 ("Points,Price -n -r"): numeric reverse sort}
                     ::::                                                                                                                      :::::::::::: :: ::
                     ::::---("-i": ignore-case)                       ("Points,Price [-n] [-r]": sort flags, "-n" (numeric), "-r" (reverse))---::::::::::::-::-::
                     :::----("-f": fields)                                                                                                     :::::::::::: :: ::
                     ::-----("-d": display)     ("Points,Price [-n] [-r]": <integer>/<fraction> sorts are "lexical" if "-n" flag is omitted)---::::::::::::-::-::                                                                                                                   :::::::::::: :: ::
                                                                           ::::::::: ::::::::::
Sample Config Files:                                                       :::::::::-::::::::::---("<decimal> fields": <integer> (ex: 1357") or <fraction> (ex: 3.14))

     # Sample "config=<config>" parm file ($config):          {Runs 01-02: Organization Membership}

          LAPTOP-MOQUDB6E:/tmp $ cat config_luna06_csv
          title="Luna Mendez Fan Club"
          rows="rows_luna06_csv"
          chart="chart_luna06"
          schema="First,Middle,Last,ID_#,Nickname,Title,Dept,Region,Country,Continent,Birthday,Ht_(ft)"
          scope="ID_#,First,Last,Birthday,Ht_(ft),Title,Nickname,Region,Country,Continent"

     # Sample "config=<config>" parm file ($config):          {Runs 03-04: Wine Industry Review}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          LAPTOP-MOQUDB6E:/tmp $ cat config_wine_equ
          delim="="
          target="schema"
          title="Wine_Review_3k"
          rows="rows_wine_equ_3k"
          chart="chart_wine_equ_3k"
          scope="ID_#,Country,Province,Winery,Variety,Points,Price,Description"
          schema="ID_#,Country,Description,Designation,Points,Price,Province,Region_1,Region_2,Taster_Name,Taster_Twitter,Title,Variety,Winery"

Sample Data Files:

     # Sample "rows=<rows>" raw data file ($rows):            {Runs 01-02: Organization Membership}

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

     # Sample "rows=<rows>" raw data file ($rows):            {Runs 03-04: Wine Industry Review}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

         *Notes: Data below from "rows_wine_equ_3k" truncated (<...>).  Complete file "dataextr_r012_(rows_wine_equ_3k).txt" (3,022 rows) @ https://github.com/bluenunn/Toolchest/
                 Sample "=" delimited wine data below may be used for Runs 03/04 by removing "<...>" & staging on Unix host along with DataExtr utility & "config_wine_equ" file.
                 Eliminate text edit line wrap on a Macbook with BBEdit @ Macbook App store or BBEdit download site: http://www.barebones.com/products/textwrangler/download.html

          LAPTOP-MOQUDB6E:/tmp $ view rows_wine_equ_3k
          1=Italy=Barbecue spice and teriyaki sauce flavors give the wine a bold, chewy feel.=Missoni=86=21=Sicily & Sardinia=Sicilia====Feudi del Pisciotto 2010 Missoni Cabernet Sauvignon (Sicilia)=Cabernet Sauvignon=Feudi del Pisciotto
          <...>
          21=Italy=Stone fruit, toasted almond and candied lemon give it heft and consistency.=Terso=89=36=Veneto=Veneto====Marchesi Fumanelli 2005 Terso White (Veneto)=White Blend=Marchesi Fumanelli
          22=Italy=Measured aromas include melon, apricot, grapefruit and a touch of vanilla spice.=Sirmian=89=26=Northeastern Italy=Alto Adige====Nals Margreid 2010 Sirmian Pinot Bianco (Alto Adige)=Pinot Bianco=Nals Margreid
          <...>
          96=France=An enticingly perfumed wine, with its white flowers and apricot blossom.=Les Belles Vignes=88=22=Loire Valley=Sancerre==Roger Voss=@vossroger=Fournier PÃ¨re et Fils 2006 Les Belles Vignes  (Sancerre)=Sauvignon Blanc=Fournier PÃ¨re et Fils
          97=Italy=Citrus, yellow apple, orange blossom and saline aromas take center stage.=Zahara=88=22=Sicily & Sardinia=Sicilia==Kerin Oâ€™Keefe=@kerinokeefe=Casa di Grazia 2013 Zahara Grillo (Sicilia)=Grillo=Casa di Grazia
          <...>
          254=US=An expert balance of tannins and acidity gives the mouth a clean wash.=Bailey Ranch Dry Farmed=90=38=California=Paso Robles=Central Coast=Matt Kettmann=@mattkettmann=Lone Madrone 2011 Bailey Ranch Dry Farmed Zinfandel (Paso Robles)=Zinfandel=Lone Madrone
          255=US=This moderately aromatic wine offers notes of pear, citrus and apricot.==88=10=Washington=Columbia Valley (WA)=Columbia Valley=Sean P. Sullivan=@wawinereport=14 Hands 2014 Riesling (Columbia Valley (WA))=Riesling=14 Hands
          <...>
          529=Austria=The merest touch of apricot caresses the vanilla and hazelnut notes of fine oak.=Kreuzgang=92=39=Wagram===Anne KrebiehlÂ MW=@AnneInVino=Anton Bauer 2015 Kreuzgang Chardonnay (Wagram)=Chardonnay=Anton Bauer
          530=France=Very international in style, it is good, but could come from anywhere.==90=70=Bordeaux=Pauillac==Roger Voss=@vossroger=ChÃ¢teau Pontet-Canet 2000  Pauillac=Bordeaux-style Red Blend=ChÃ¢teau Pontet-Canet
          <...>
          587=US=The foam is quite mouthfilling, but lemony acidity leads to a lingering finish.=Blanc de Blancs=88=42=New York=North Fork of Long Island=Long Island=Anna Lee C. Iijima==Sparkling Pointe 2005 Blanc de Blancs Sparkling (North Fork of Long Island)=Sparkling Blend=Sparkling Pointe
          588=US=It's dry and brisk, with pleasant apricot, tangerine and mineral flavors.==88=17=California=Napa Valley=Napa===Educated Guess 2009 Chardonnay (Napa Valley)=Chardonnay=Educated Guess
          <...>
          1416=US=Striking notes of ripe peach, apricot and white lily carry the nose and palate.=Timmons Ranch=88=13=Texas=Texas==Alexander Peartree==McPherson 2015 Timmons Ranch Piquepoul Blanc (Texas)=Piquepoul Blanc=McPherson
          1417=US=The acidity and body are both medium level, while the ripeness is high.=Black Label=88=59=California=Russian River Valley=Sonoma=Virginie Boone=@vboone=Roadhouse Winery 2014 Black Label Pinot Noir (Russian River Valley)=Pinot Noir=Roadhouse Winery
          <...>
          2138=US=It's a tart and vivid wine, with lovely concentration and no rough edges.=Stillwater Creek Vineyard=91=23=Washington=Columbia Valley (WA)=Columbia Valley=Paul Gregutt=@paulgwineÂ =Novelty Hill 2012 Stillwater Creek Vineyard Viognier (Columbia Valley (WA))=Viognier=Novelty Hill
          2139=US=Apricot, papaya, pineapple and banana are all here, with supporting acidity.=Oriana=91=22=Washington=Yakima Valley=Columbia Valley=Paul Gregutt=@paulgwineÂ =Brian Carter Cellars 2012 Oriana White (Yakima Valley)=White Blend=Brian Carter Cellars
          <...>
          2294=US=There's a lot of appeal with notes of apricot, mango, butter and toasty spices.==88=15=Washington=Wahluke Slope=Columbia Valley=Sean P. Sullivan=@wawinereport=Seven Falls 2011 Chardonnay (Wahluke Slope)=Chardonnay=Seven Falls
          2295=US=Drink this light-bodied, savory young Pinot now and over the next year or two.=Heritage Selection=88=55=California=Sonoma Coast=Sonoma===Gundlach Bundschu 2010 Heritage Selection Pinot Noir (Sonoma Coast)=Pinot Noir=Gundlach Bundschu
          <...>
          2492=US=Rusty in color, it offers cranberry, strawberry, sagebrush and iron aromas.=Taylor Noelle Pence Ranch Vineyard Clone 459=93=35=California=Santa Barbara County=Central Coast=Matt Kettmann=@mattkettmann=Seal Beach 2013 Taylor Noelle Pence Ranch Vineyard Clone 459 Pinot Noir (Santa Barbara County)=Pinot Noir=Seal Beach
          2493=US=The aromas are exuberant, with notes of apricot, jasmine and honeysuckle.=Underwood Mountain Vineyard=90=22=Washington=Columbia Gorge (WA)=Washington Other=Sean P. Sullivan=@wawinereport=Savage Grace 2015 Underwood Mountain Vineyard Riesling (Columbia Gorge (WA))=Riesling=Savage Grace
          <...>
          2580=US=Fleshy pear, apricot and tropical fruits are set against juicy acidity.=Crater View Vineyard=90=26=Oregon=Rogue Valley=Southern Oregon=Paul Gregutt=@paulgwineÂ =Dobbes Family Estate 2014 Crater View Vineyard Grenache Blanc (Rogue Valley)=Grenache Blanc=Dobbes Family Estate
          2581=Portugal=All about fruit, this is a ready-to-drink Port, although it could also age.=Unfiltered Late Bottled Vintage=90=26=Port===Roger Voss=@vossroger=Quinta do Noval 2009 Unfiltered Late Bottled Vintage  (Port)=Port=Quinta do Noval
          <...>
          2644=US=This is a fruity, dry rosÃ© comprised of a blend of Chardonnay and Pinot Noir.=r=89=35=Oregon=Oregon=Oregon Other=Paul Gregutt=@paulgwineÂ =Domaine Serene NV r RosÃ© (Oregon)=RosÃ©=Domaine Serene
          2645=US=This wine has a deep-gold color and aromas like apricots and toasted almonds.=Sur Lies=89=35=California=Sierra Foothills=Sierra Foothills=Jim Gordon=@gordone_cellars=Wise Villa 2014 Sur Lies Chardonnay (Sierra Foothills)=Chardonnay=Wise Villa
          <...>
          2786=Spain=For straightforward apricot and peach flavors, this wine serves its purpose.=Miguel Asensio Vino Dulce de Moscatel=88=22=Spain Other=Spain==Michael Schachner=@wineschach=Asensio 2006 Miguel Asensio Vino Dulce de Moscatel Moscatel (Spain)=Moscatel=Asensio
          2787=US=It is perfectly balanced and just slightly sweet, from fruit more than sugar.==87=13=Oregon=Oregon=Oregon Other=Paul Gregutt=@paulgwineÂ =A to Z 2006 Pinot Blanc (Oregon)=Pinot Blanc=A to Z
          <...>
          3022=US=Bring on the burgers and veal parmesan for this dry, exuberant table wine.==85=18=California=Mendocino County====Jacuzzi 2007 Barbera (Mendocino County)=Barbera=Jacuzzi

Sample Runs:

     Run01 (sort="first,last")        {Organization Membership: <alpha> lexical forward sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -di config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last"

          DataExtr Audit Complete: 2020-05-14_14.07.18_PDT.     {Elapsed (sec): 2}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 21, #_rec_output ($chart): 9, %_rec_match: 42.9%}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Config  File  ($config): /tmp/config_luna06_csv

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          15    Pocahantas  [TBD]     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America

     Run02 (sort="first,last -r")        {Organization Membership: <alpha> lexical reverse sort}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -di config=config_luna06_csv query='id_#>=10 && continent~north_America || last~mendez' sort="first,last -r"

          DataExtr Audit Complete: 2020-05-14_14.07.42_PDT.     {Elapsed (sec): 1}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 21, #_rec_output ($chart): 9, %_rec_match: 42.9%}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Config  File  ($config): /tmp/config_luna06_csv

          DataExtr Input   File    ($rows): /tmp/rows_luna06_csv

          DataExtr Output  File   ($chart): /tmp/chart_luna06

          Luna Mendez Fan Club:

          ID_#  First       Last      Birthday    Ht_(ft)  Title               Nickname               Region            Country  Continent
          16    Wiley       Coyote    1975_03-10  4.021    Roadrunner_Nemesis  Coyote_Sventato        La_Paz            Mexico   North_America
          19    Speedy      Gonzales  1967_07-12  0.48     Cats_Meow           Mouse_Molto_Veloce     Guadalajara       Mexico   North_America
          10    Road        Runner    1976_07-04  2.87     Coyote_Nemesis      Uccello_Veloce         La_Paz            Mexico   North_America
          15    Pocahantas  [TBD]     1999_07-07  05.730   Disney_Princess     Fanciulla_Indiana      Enchanted_Forest  USA      North_America
          2     Max         Mendez    1993_06-30  5.6      Knight              Muscoli_a_Bizzeffe     Sicily            Italy    Europe
          1     Luna        Mendez    2019_05-31  0.512    Princess            Zampe_Bianche          Calabria          Italy    Europe
          14    Elmer       Fudd      1958_05-16  5.01     Bugs_Nemesis        Cacciatore_di_Conigli  Carrot_Patch      USA      North_America
          20    Daffy       Duck      1965_04-25  1.27     Zany_Mallard        Anatra_Allegra         Carrot_Patch      USA      North_America
          21    Bugs        Bunny     1962_08-19  1.3      Elmer_Nemesis       Coniglio_di_Carota     Carrot_Patch      USA      North_America

     Run03 (sort="id_# -n")          {Wine Industry Review: <integer> numeric forward sort}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -di config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n"

          DataExtr Audit Complete: 2020-05-14_14.50.31_PDT.     {Elapsed (sec): 2}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 3022, #_rec_output ($chart): 12, %_rec_match: 0.4%}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Config  File  ($config): /tmp/config_wine_equ

          DataExtr Input   File    ($rows): /tmp/rows_wine_equ_3k

          DataExtr Output  File   ($chart): /tmp/chart_wine_equ_3k

          Wine_Review_3k:

          ID_#  Country  Province            Winery                  Variety          Points  Price  Description
          1416  US       Texas               McPherson               Piquepoul_Blanc  88      13     Striking_notes_of_ripe_peach,_apricot_and_white_lily_carry_the_nose_and_palate.
          2294  US       Washington          Seven_Falls             Chardonnay       88      15     There's_a_lot_of_appeal_with_notes_of_apricot,_mango,_butter_and_toasty_spices.
          255   US       Washington          14_Hands                Riesling         88      10     This_moderately_aromatic_wine_offers_notes_of_pear,_citrus_and_apricot.
          2786  Spain    Spain_Other         Asensio                 Moscatel         88      22     For_straightforward_apricot_and_peach_flavors,_this_wine_serves_its_purpose.
          588   US       California          Educated_Guess          Chardonnay       88      17     It's_dry_and_brisk,_with_pleasant_apricot,_tangerine_and_mineral_flavors.
          96    France   Loire_Valley        Fournier_PÃ¨re_et_Fils  Sauvignon_Blanc  88      22     An_enticingly_perfumed_wine,_with_its_white_flowers_and_apricot_blossom.
          22    Italy    Northeastern_Italy  Nals_Margreid           Pinot_Bianco     89      26     Measured_aromas_include_melon,_apricot,_grapefruit_and_a_touch_of_vanilla_spice.
          2645  US       California          Wise_Villa              Chardonnay       89      35     This_wine_has_a_deep-gold_color_and_aromas_like_apricots_and_toasted_almonds.
          2493  US       Washington          Savage_Grace            Riesling         90      22     The_aromas_are_exuberant,_with_notes_of_apricot,_jasmine_and_honeysuckle.
          2580  US       Oregon              Dobbes_Family_Estate    Grenache_Blanc   90      26     Fleshy_pear,_apricot_and_tropical_fruits_are_set_against_juicy_acidity.
          2139  US       Washington          Brian_Carter_Cellars    White_Blend      91      22     Apricot,_papaya,_pineapple_and_banana_are_all_here,_with_supporting_acidity.
          529   Austria  Wagram              Anton_Bauer             Chardonnay       92      39     The_merest_touch_of_apricot_caresses_the_vanilla_and_hazelnut_notes_of_fine_oak.

     Run04 (sort="id_# -n -r")          {Wine Industry Review: <integer> numeric reverse sort}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          LAPTOP-MOQUDB6E:/tmp $ ./dataextr -dif config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n -r"

          DataExtr Audit Complete: 2020-05-14_14.08.45_PDT.     {Elapsed (sec): 3}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 3022, #_rec_output ($chart): 12, %_rec_match: 0.4%}

          DataExtr Stage Directory  ($dir): /tmp

          DataExtr History File ($history): /tmp/dataextr_history

          DataExtr Config  File  ($config): /tmp/config_wine_equ

          DataExtr Input   File    ($rows): /tmp/rows_wine_equ_3k

          DataExtr Output  File   ($chart): /tmp/chart_wine_equ_3k

          Wine_Review_3k:

          ID_#  Country  Province            Winery                  Variety          Points  Price  Description
          529   Austria  Wagram              Anton_Bauer             Chardonnay       92      39     The_merest_touch_of_apricot_caresses_the_vanilla_and_hazelnut_notes_of_fine_oak.
          2139  US       Washington          Brian_Carter_Cellars    White_Blend      91      22     Apricot,_papaya,_pineapple_and_banana_are_all_here,_with_supporting_acidity.
          2580  US       Oregon              Dobbes_Family_Estate    Grenache_Blanc   90      26     Fleshy_pear,_apricot_and_tropical_fruits_are_set_against_juicy_acidity.
          2493  US       Washington          Savage_Grace            Riesling         90      22     The_aromas_are_exuberant,_with_notes_of_apricot,_jasmine_and_honeysuckle.
          2645  US       California          Wise_Villa              Chardonnay       89      35     This_wine_has_a_deep-gold_color_and_aromas_like_apricots_and_toasted_almonds.
          22    Italy    Northeastern_Italy  Nals_Margreid           Pinot_Bianco     89      26     Measured_aromas_include_melon,_apricot,_grapefruit_and_a_touch_of_vanilla_spice.
          96    France   Loire_Valley        Fournier_PÃ¨re_et_Fils  Sauvignon_Blanc  88      22     An_enticingly_perfumed_wine,_with_its_white_flowers_and_apricot_blossom.
          588   US       California          Educated_Guess          Chardonnay       88      17     It's_dry_and_brisk,_with_pleasant_apricot,_tangerine_and_mineral_flavors.
          2786  Spain    Spain_Other         Asensio                 Moscatel         88      22     For_straightforward_apricot_and_peach_flavors,_this_wine_serves_its_purpose.
          255   US       Washington          14_Hands                Riesling         88      10     This_moderately_aromatic_wine_offers_notes_of_pear,_citrus_and_apricot.
          2294  US       Washington          Seven_Falls             Chardonnay       88      15     There's_a_lot_of_appeal_with_notes_of_apricot,_mango,_butter_and_toasty_spices.
          1416  US       Texas               McPherson               Piquepoul_Blanc  88      13     Striking_notes_of_ripe_peach,_apricot_and_white_lily_carry_the_nose_and_palate.

Sample History Files ($history):

     Run03 (sort="id_# -n")             {Wine Industry Review: <integer> numeric forward sort}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          LAPTOP-MOQUDB6E:/tmp $ cat dataextr_history       # {Run 03 DataExtr "-f" (fields) option not set, reduces run overhead ("2 vs 3" sec, see Run 04 History file)}

          DataExtr History File: /tmp/dataextr_history

          Audit Timestamp (start):  2020-05-14_14.50.29_PDT

          Command Line: ./dataextr -di config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n"

          Data_Source: LAPTOP-MOQUDB6E      {User_ID: bluenunn, OS: Ubuntu 18.04.1 LTS}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 3022, #_rec_output ($chart): 12, %_rec_match: 0.4%}

          Commands:

               RC: 0     Command:  sort -t = -k6,7 -n /tmp/datatmp_chart06_jus > /tmp/datatmp_chart07_srt

          DataExtr Stage Directory ($dir): /tmp

          DataExtr Config File  ($config): /tmp/config_wine_equ

          DataExtr Input  File    ($rows): /tmp/rows_wine_equ_3k

          DataExtr Output File   ($chart): /tmp/chart_wine_equ_3k

          Audit Timestamp (finish): 2020-05-14_14.50.31_PDT     {Elapsed (sec): 2}}

     Run04 (sort="id_# -n -r")          {Wine Industry Review: <integer> numeric reverse sort}          {Data_Source: https://www.kaggle.com/zynicide/wine-reviews}

          LAPTOP-MOQUDB6E:/tmp $ cat dataextr_history       # {Run 04 DataExtr "-f" (fields) option set, increases run overhead ("3 vs 2" sec, see Run 03 History file)}

          DataExtr History File: /tmp/dataextr_history

          Audit Timestamp (start):  2020-05-14_14.08.42_PDT

          Command Line: ./dataextr -dif config=config_wine_equ query='Description~apricot && points>=88 && price<=40' sort="Points,Price -n -r"

          Data_Source: LAPTOP-MOQUDB6E      {User_ID: bluenunn, OS: Ubuntu 18.04.1 LTS}

          DataExtr Return Code: 0           {"0": no errors found, "<non-zero>": errors detected (check History file)}

          DataExtr Audit Overview:          {#_rec_input ($rows): 3022, #_rec_output ($chart): 12, %_rec_match: 0.4%}

          Option Settings:      {"0": false, "1": true}     {Option Sources:  (def): default, (cli): CLI}

               "-d" (cli):  1   {display}

               "-f" (cli):  1   {fields}

               "-i" (cli):  1   {ignore-case}

               "-r" (def):  0   {raw}

               "-t" (def):  0   {temporary}

          Parm Values:          {Parm Sources:  (def): default, (cfg): <cfg_file>, (cli): CLI, (sch): scope="$schema"}

                   "dir" (def):  dir=/tmp

                "config" (cli):  config=config_wine_equ

                 "title" (cfg):  title=Wine_Review_3k

                 "delim" (cfg):  delim="="

               "history" (def):  history=dataextr_history

                  "rows" (cfg):  rows=rows_wine_equ_3k

                 "chart" (cfg):  chart=chart_wine_equ_3k

                "schema" (cfg):  schema=ID_#,Country,Description,Designation,Points,Price,Province,Region_1,Region_2,Taster_Name,Taster_Twitter,Title,Variety,Winery

                 "scope" (cfg):  scope=ID_#,Country,Province,Winery,Variety,Points,Price,Description

                "target" (cfg):  target=schema

                 "query" (cli):  query='Description~apricot && points>=88 && price<=40'

               "include" (def):

               "exclude" (def):

                  "sort" (cli):  sort="Points,Price -n -r"

          Commands:

               RC: 0     Command:  sort -t = -k6,7 -n -r /tmp/datatmp_chart06_jus > /tmp/datatmp_chart07_srt

          DataExtr Stage Directory ($dir): /tmp

          DataExtr Config File  ($config): /tmp/config_wine_equ

          DataExtr Input  File    ($rows): /tmp/rows_wine_equ_3k

          DataExtr Output File   ($chart): /tmp/chart_wine_equ_3k

          Audit Timestamp (finish): 2020-05-14_14.08.45_PDT     {Elapsed (sec): 3}}
