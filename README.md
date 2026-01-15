# utl-altair-slc-sqlite-odbc-reading-and-writing-tables-using-passthru-processing
Altair slc sqlite odbc reading and writing tables using passthru processing
    %let pgm=utl-altair-slc-sqlite-odbc-reading-and-writing-tables-using-passthru-processing;

    %stop_processing;

    Altair slc sqlite odbc reading and writing tables using passthru processing

    Too long to post here, see github
    github
    https://github.com/rogerjdeangelis/utl-altair-slc-sqlite-odbc-reading-and-writing-tables-using-passthru-processing

    SQLite's load_extension() uniquely allows dynamic loading of over 200 specialized
    functions not available in PostgreSQL, MySQL, SQL Server, or Oracle ...:

    Sqlite is a simple file database, no need for userid, password, connections strings or
    all the overhead of typical databases. This makes it especially useful to
    mimic 'proc sql' adding substantial new fuctionality to sql. Expecially
    sql clauses like 'partition by'.

    This post allows you to use the exact same very advanced sqlite scripts in
    any language that supports odbc.
    Eliminates the ankle biters created by slight differences in sql dialects.

    SQLITE CAN BE USED WITH

       Altair SLC
       Pyhton
       R
       Excel
       Octave (Matlab clone)
       PSPP (SPSS clone)
       Stata
       MS Access
       ....

    CONTENTS

     1. sqlite create csv
     2. sqlite get extensions
     3. get odbc driver
     4. create sqlite db & table
     5. sqlite passthru
     6. output
     7. log
     8. extensions list

    /*             _ _ _                             _
    / |  ___  __ _| (_) |_ ___    ___ _ __ ___  __ _| |_ ___    ___ _____   __
    | | / __|/ _` | | | __/ _ \  / __| `__/ _ \/ _` | __/ _ \  / __/ __\ \ / /
    | | \__ \ (_| | | | ||  __/ | (__| | |  __/ (_| | ||  __/ | (__\__ \\ V /
    |_| |___/\__, |_|_|\__\___|  \___|_|  \___|\__,_|\__\___|  \___|___/ \_/
                |_|
    */

    EXAMPLE OF ADVANCED FUNCTIONALITY ( CREATING A CSV FILE FROM A SQLITE TABLE )
    -----------------------------------------------------------------------------

    INPUT SQLITE TABLE
    ------------------'

    CLASS_SQLITE

    NAME     SEX

    Alfred    M
    Alice     F
    Barbara   F
    Carol     F
    Henry     M
    James     M

    PROCESS TO CREATE CSV FILE
    --------------------------

    proc sql;
      connect to odbc(dsn="wernerdsn");
      select * from connection to odbc (
        SELECT writefile('c:/temp/names.txt',
          replace(
            group_concat(printf('%s,%s', NAME, SEX), char(13)||char(10)),
            char(13)||char(10)||'$', ''
          )
        ) AS result
        FROM class_sqlite
        ORDER BY NAME
      );
      disconnect from odbc;
    quit;

    OUTPUT TEXT FILE
    ----------------

    c:/temp/names.txt

    Alfred,M
    Alice,F
    Barbara,F
    Carol,F
    Henry,M
    James,M

    /*___              _ _ _                    _              _                 _
    |___ \   ___  __ _| (_) |_ ___    __ _  ___| |_   _____  _| |_ ___ _ __  ___(_) ___  _ __  ___
      __) | / __|/ _` | | | __/ _ \  / _` |/ _ \ __| / _ \ \/ / __/ _ \ `_ \/ __| |/ _ \| `_ \/ __|
     / __/  \__ \ (_| | | | ||  __/ | (_| |  __/ |_ |  __/>  <| ||  __/ | | \__ \ | (_) | | | \__ \
    |_____| |___/\__, |_|_|\__\___|  \__, |\___|\__| \___/_/\_\\__\___|_| |_|___/_|\___/|_| |_|___/
                    |_|              |___/

    SQLITE EXTENSIONS
    -----------------

     1. Create folder  d:/dll

     2. possible downloads
        https://github.com/nalgeon/sqlean
        https://github.com/rogerjdeangelis/utl-altair-slc-sqlite-odbc-reading-and-writing-tables-using-passthru-processing/blob/main/sqlean.dll
        https://github.com/rogerjdeangelis/utl-altair-slc-enhanced-proc-sql-like-processing-inside-python-and-r/blob/main/sqlean.dll
        https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories/blob/master/sqlean.dll

     3. Place sqlean.dll in
        d:/dll/sqlean.dll

     _____              _               _ _                _      _
    |___ /    __ _  ___| |_    ___   __| | |__   ___    __| |_ __(_)_   _____ _ __
      |_ \   / _` |/ _ \ __|  / _ \ / _` | `_ \ / __|  / _` | `__| \ \ / / _ \ `__|
     ___) | | (_| |  __/ |_  | (_) | (_| | |_) | (__  | (_| | |  | |\ V /  __/ |
    |____/   \__, |\___|\__|  \___/ \__,_|_.__/ \___|  \__,_|_|  |_| \_/ \___|_|
             |___/

     CHRISTIAN WERNER ODBC DRIVER (THIS IS A SYSTEM ODBC DRIVER)
     -----------------------------------------------------------
     1. Download Christian Werner ODBC Drive (sqliteodbc_w64)
        Note some browsers will block the download as unsafe mainly because
        the site is http and not https. I have tested the download for malicious software and
        it is ok, however always 'create a restore point' before downloading.

        http://www.ch-werner.de/sqliteodbc/
        https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories
        https://github.com/rogerjdeangelis/utl-altair-slc-sqlite-odbc-reading-and-writing-tables-using-passthru-processing/blob/main/sqliteodbc_w64.exe

     2. I assigned DSN 'wernerdsn' and tied it to database 'd:/sqlite/mysqlite.db'.
        You need to use Windows 'Data Source Admistator (64bit)' and a system DSN options.
        Also create dsn and path to sqlite file.

        In the SQLite3 ODBC Configuation Panel Enter

           Data Source:    Name wernerdsn
           Database Name:  d:/sqlite/mysqlite.db

           Load Extension: d:/dll/sqlean.dll

    /*  _                         _             _ _       ___    _        _     _
    | || |     ___ _ __ ___  __ _| |_ ___    __| | |__   ( _ )  | |_ __ _| |__ | | ___
    | || |_   / __| `__/ _ \/ _` | __/ _ \  / _` | `_ \  / _ \/\| __/ _` | `_ \| |/ _ \
    |__   _| | (__| | |  __/ (_| | ||  __/ | (_| | |_) || (_>  <| || (_| | |_) | |  __/
       |_|    \___|_|  \___|\__,_|\__\___|  \__,_|_.__/  \___/\/ \__\__,_|_.__/|_|\___|

    */

    options validvarname=upcase;
    libname workx sas7bdat "d:/wpswrkx";
    data workx.class_sqlite;
      input
        name$
        sex$ age;
    cards4;
    Alfred  M 14
    Alice   F 13
    Barbara F 13
    Carol   F 14
    Henry   M 14
    James   M 12
    ;;;;
    run;quit;

    /*---
    WORKX.CLASS_SQLITE total obs=6
    bs     NAME      SEX    AGE

    1     Alfred      M      14
    2     Alice       F      13
    3     Barbara     F      13
    4     Carol       F      14
    5     Henry       M      14
    6     James       M      12
    ---*/

    /*--- CREATE SQLITE DATABASE D:/SQLITE/MYSQLITE.DB WITH TABLE CLASS_SQLITE ---*/

    options set=RHOME "C:\Progra~1\R\R-4.5.2\bin\r";
    proc r;
    submit;
    library(haven)
    library(DBI)
    library(RSQLite)
    class_sqlite<-read_sas("d:/wpswrkx/class_sqlite.sas7bdat");
    class_sqlite
    # Connect to (or create) SQLite DB file
    con <- dbConnect(RSQLite::SQLite(), "d:/sqlite/mysqlite.db")

    # Load and write class_sqlite (overwrites if table exists)
    dbWriteTable(con, "class_sqlite", class_sqlite, overwrite = TRUE)

    # Verify: preview data
    head(dbReadTable(con, "class_sqlite"))

    # Close connection
    dbDisconnect(con)
    endsubmit;
    run;quit;

    /*___              _ _ _                             _   _
    | ___|   ___  __ _| (_) |_ ___   _ __   __ _ ___ ___| |_| |__  _ __ _   _
    |___ \  / __|/ _` | | | __/ _ \ | `_ \ / _` / __/ __| __| `_ \| `__| | | |
     ___) | \__ \ (_| | | | ||  __/ | |_) | (_| \__ \__ \ |_| | | | |  | |_| |
    |____/  |___/\__, |_|_|\__\___| | .__/ \__,_|___/___/\__|_| |_|_|   \__,_|
                    |_|             |_|
    */


    options ls=255;

    /*-------------------------------------------------------------------------*/
    /*--- PASSTRU TO SQLITE AND SHOW SQLITE SUPERIOR FUNCTIONALITY          ---*/
    /*-------------------------------------------------------------------------*/

    libname sqlite odbc dsn="wernerdsn";

    /*--- CONTENTS OF SQLITE DATABASE ---*/
    proc contents data=sqlite.class_sqlite;
    run;quit;

    /*--- PRINT CLASS_SQLITE INSIDE DATABASE ---*/
    proc print data=sqlite.class_sqlite width=min;
    title "Table class_from_sqlite";
    run;quit;

    libname sqlite clear;

    /* --- PASSTHRU HERE WE CAN USE FUNCTIONALITY NOT AVAILABLE IN PROC SQL ---*/
    proc sql;
      connect to odbc(dsn="wernerdsn");
      create table class_names_sas7bdat as
      select * from connection to odbc (
        SELECT sex, GROUP_CONCAT(name) AS names_list
        FROM class_sqlite
        GROUP BY sex
      );
      disconnect from odbc;
    quit;

    libname sqlite odbc dsn="wernerdsn";

    /*--- PRINT RESULT OF SQLITE ENHANCED FUNTIONALITY ---*/
    proc print data=class_names_sas7bdat width=min;
    title "PRINT RESULT OF SQLITE GROUP_CAT NOT AVAILABLE IN SAS";
    run;quit;

    /*-------------------------------------------------------------------------*/
    /*--- CREATE SAS DATASET FROM SQLITE TABLE                              ---*/
    /*-------------------------------------------------------------------------*/

    /*--- CREATE SAS7BDAT FROM SQLITE.CLASS_SQLITE ---*/
    data class_sas7bdat;
      set sqlite.class_sqlite;
    run;quit;

    /*--- PRINT SAS/SLC DATASET EXPORTED FROM SQLITE TABLE ---*/
    proc print data=class_sas7bdat width=min;
    title "SAS/SLC DATASET EXPORTED FROM SQLITE TABLE";
    run;quit;

    /*-------------------------------------------------------------------------*/
    /*--- CREATE TABLES IN SQLITE FROM SAS/SLC DATASETS                     ---*/
    /*-------------------------------------------------------------------------*/

    proc sql;
      connect to odbc(dsn="wernerdsn");
      execute('DROP TABLE IF EXISTS class_names_sqlite') by odbc;
      disconnect from odbc;
    quit;

    libname sqlite odbc dsn="wernerdsn";

    data sqlite.class_names_sqlite;
      set class_names_sas7bdat;
    run;quit;

    proc print data=sqlite.class_names_sqlite width=minimum;
    title "WRITE SAS DATASET CLASS_NAMES_SAS7BDAT INTO SQL FILE DATABASE D:/SQLITE/MYSQLITE.DB CLASS_NAMES_SQLITE";
    run;quit;

    libname sqlite clear;

    run;quit;

    /*__                 _               _
     / /_     ___  _   _| |_ _ __  _   _| |_
    | `_ \   / _ \| | | | __| `_ \| | | | __|
    | (_) | | (_) | |_| | |_| |_) | |_| | |_
     \___/   \___/ \__,_|\__| .__/ \__,_|\__|
                            |_|
    */

    /*---

    PROC CONTENTS ON THE SQLITE TABLE CLASS_SQLITE IN FILE DATABASE  D:/SQLITE/MYSQLITE.DB
    --------------------------------------------------------------------------------------

    Altair SLC
    LIST: 13:59:33

    Altair SLC

    The CONTENTS Procedure

    Data Set Name           CLASS_SQLITE
    Member Type             VIEW
    Engine
    Observations                .
    Variables               3
    Indexes                 0
    Observation Length      2056
    Deleted Observations             0
    Data Set Type
    Label
    Compressed              NO
    Sorted                  NO
    Data Representation
    Encoding                wlatin1 Windows-1252 Western

                              Alphabetic List of Variables and Attributes

          Number    Variable    Type             Len             Pos    Format          Informat
    ________________________________________________________________________________________________
               3    AGE         Num                8            2048
               1    NAME        Char            1024               0    $1024.          $1024.
               2    SEX         Char            1024            1024    $1024.          $1024.


    DIRECT LISTING OF SQLITE TABLE IN D:/SQLITE/MYSQLITE.DB WITHOUT COVERRTING TO A SAS DATASET
    -------------------------------------------------------------------------------------------


    Table class_from_sqlite (VIEW OD SQLITE TABLE)

      Obs   NAME      SEX    AGE

       1    Alfred      M      14
       2    Alice       F      13
       3    Barbara     F      13
       4    Carol       F      14
       5    Henry       M      14
       6    James       M      12


    USING AVANCED SQL SQLITE FUNCTION TO CONCATENATE NAMES BY SEX AND OUTPUT SAS DATASET
    ------------------------------------------------------------------------------------

    SAS DATASET CLASS_NAMES_SAS7BDAT

    Obs    SEX        NAMES_LIST

     1      F     Alice,Barbara,Carol
     2      M     Alfred,Henry,James


    WRITE SAS DATASET TO D:/SQLITE/MYSQLITE.DB DATABASE
    ---------------------------------------------------

    WRITE SAS DATASET CLASS_NAMES_SAS7BDAT INTO SQL FILE DATABASE D:/SQLITE/MYSQLITE.DB CLASS_NAMES_SQLITE

      Obs   SEX        NAMES_LIST

       1     F     Alice,Barbara,Carol
       2     M     Alfred,Henry,James

    /*____   _
    |___  | | | ___   __ _
       / /  | |/ _ \ / _` |
      / /   | | (_) | (_| |
     /_/    |_|\___/ \__, |
                     |___/
    */

    1                                          Altair SLC      11:49 Thursday, January 15, 2026

    NOTE: Copyright 2002-2025 World Programming, an Altair Company
    NOTE: Altair SLC 2026 (05.26.01.00.000758)
          Licensed to Roger DeAngelis
    NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

    NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
    NOTE: AUTOEXEC source line
    1       +  ï»¿ods _all_ close;
               ^
    ERROR: Expected a statement keyword : found "?"
    NOTE: Library workx assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\wpswrkx

    NOTE: Library slchelp assigned as follows:
          Engine:        WPD
          Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp


    LOG:  11:49:26
    NOTE: 1 record was written to file PRINT

    NOTE: The data step took :
          real time : 0.031
          cpu time  : 0.000


    NOTE: AUTOEXEC processing completed

    1         options ls=255;
    2
    3         /*-------------------------------------------------------------------------*/
    4         /*--- PASSTRU TO SQLITE AND SHOW SQLITE SUPERIOR FUNCTIONALITY          ---*/
    5         /*-------------------------------------------------------------------------*/
    6
    7         libname sqlite odbc dsn="wernerdsn";
    NOTE: Library sqlite assigned as follows:
          Engine:        ODBC
          Physical Name: wernerdsn (SQLite version 3.43.2)

    8
    9         /*--- CONTENTS OF SQLITE DATABASE ---*/
    10        proc contents data=sqlite.class_sqlite;
    11        run;quit;
    WARNING: truncating character column NAME to 1024 characters long, based on dbmax_text setting.
    WARNING: truncating character column SEX to 1024 characters long, based on dbmax_text setting.
    NOTE: Procedure contents step took :
          real time : 0.031
          cpu time  : 0.000


    WARNING: truncating character column NAME to 1024 characters long, based on dbmax_text setting.
    WARNING: truncating character column SEX to 1024 characters long, based on dbmax_text setting.
    12
    13        /*--- PRINT CLASS_SQLITE INSIDE DATABASE ---*/
    14        proc print data=sqlite.class_sqlite width=min;
    15        title "Table class_from_sqlite";
    16        run;quit;
    WARNING: truncating character column NAME to 1024 characters long, based on dbmax_text setting.
    WARNING: truncating character column SEX to 1024 characters long, based on dbmax_text setting.
    NOTE: 6 observations were read from "SQLITE.class_sqlite"
    NOTE: Procedure print step took :
          real time : 0.063

    2                                                                                                                         Altair SLC

          cpu time  : 0.015


    NOTE: Libref SQLITE has been deassigned.
    17
    18        libname sqlite clear;
    19
    20        /* --- PASSTHRU HERE WE CAN USE FUNCTIONALITY NOT AVAILABLE IN PROC SQL ---*/
    21        proc sql;
    22          connect to odbc(dsn="wernerdsn");
    NOTE: Connected to DB: wernerdsn (SQLite version 3.43.2)
    NOTE: Connected to DB: wernerdsn (SQLite version 3.43.2)
    NOTE: Successfully connected to database ODBC as alias ODBC.
    23          create table class_names_sas7bdat as
    24          select * from connection to odbc (
    25            SELECT sex, GROUP_CONCAT(name) AS names_list
    26            FROM class_sqlite
    27            GROUP BY sex
    28          );
    WARNING: truncating character column SEX to 1024 characters long, based on dbmax_text setting.
    NOTE: Data set "WORK.class_names_sas7bdat" has 2 observation(s) and 2 variable(s)
    29          disconnect from odbc;
    NOTE: Successfully disconnected from database ODBC.
    30        quit;
    NOTE: Procedure sql step took :
          real time : 0.015
          cpu time  : 0.015


    31
    32        libname sqlite odbc dsn="wernerdsn";
    NOTE: Library sqlite assigned as follows:
          Engine:        ODBC
          Physical Name: wernerdsn (SQLite version 3.43.2)

    33
    34        /*--- PRINT RESULT OF SQLITE ENHANCED FUNTIONALITY ---*/
    35        proc print data=class_names_sas7bdat width=min;
    36        title "PRINT RESULT OF SQLITE GROUP_CAT NOT AVAILABLE IN SAS";
    37        run;quit;
    NOTE: 2 observations were read from "WORK.class_names_sas7bdat"
    NOTE: Procedure print step took :
          real time : 0.015
          cpu time  : 0.015


    38
    39        /*-------------------------------------------------------------------------*/
    40        /*--- CREATE SAS DATASET FROM SQLITE TABLE                              ---*/
    41        /*-------------------------------------------------------------------------*/
    42
    43        /*--- CREATE SAS7BDAT FROM SQLITE.CLASS_SQLITE ---*/
    44        data class_sas7bdat;
    WARNING: truncating character column NAME to 1024 characters long, based on dbmax_text setting.
    WARNING: truncating character column SEX to 1024 characters long, based on dbmax_text setting.
    45          set sqlite.class_sqlite;
    46        run;

    NOTE: 6 observations were read from "SQLITE.class_sqlite"
    NOTE: Data set "WORK.class_sas7bdat" has 6 observation(s) and 3 variable(s)
    NOTE: The data step took :
          real time : 0.000
          cpu time  : 0.015

    3                                                                                                                         Altair SLC



    46      !     quit;
    47
    48        /*--- PRINT SAS/SLC DATASET EXPORTED FROM SQLITE TABLE ---*/
    49        proc print data=class_sas7bdat width=min;
    50        title "SAS/SLC DATASET EXPORTED FROM SQLITE TABLE";
    51        run;quit;
    NOTE: 6 observations were read from "WORK.class_sas7bdat"
    NOTE: Procedure print step took :
          real time : 0.015
          cpu time  : 0.015


    52
    53        /*-------------------------------------------------------------------------*/
    54        /*--- CREATE TABLES IN SQLITE FROM SAS/SLC DATASETS                     ---*/
    55        /*-------------------------------------------------------------------------*/
    56
    57        proc sql;
    58          connect to odbc(dsn="wernerdsn");
    NOTE: Connected to DB: wernerdsn (SQLite version 3.43.2)
    NOTE: Connected to DB: wernerdsn (SQLite version 3.43.2)
    NOTE: Successfully connected to database ODBC as alias ODBC.
    59          execute('DROP TABLE IF EXISTS class_names_sqlite') by odbc;
    NOTE: Successfully passed statement to database ODBC.
    60          disconnect from odbc;
    NOTE: Successfully disconnected from database ODBC.
    61        quit;
    NOTE: Procedure sql step took :
          real time : 0.078
          cpu time  : 0.015


    62
    63        libname sqlite odbc dsn="wernerdsn";
    NOTE: Library sqlite assigned as follows:
          Engine:        ODBC
          Physical Name: wernerdsn (SQLite version 3.43.2)

    64
    65        data sqlite.class_names_sqlite;
    66          set class_names_sas7bdat;
    67        run;

    NOTE: 2 observations were read from "WORK.class_names_sas7bdat"
    NOTE: Data set "SQLITE.class_names_sqlite" has an unknown number of observation(s) and 2 variable(s)
    NOTE: The data step took :
          real time : 0.127
          cpu time  : 0.015


    67      !     quit;
    68
    69        proc print data=sqlite.class_names_sqlite width=minimum;
    70        title "WRITE SAS DATASET CLASS_NAMES_SAS7BDAT INTO SQL FILE DATABASE D:/SQLITE/MYSQLITE.DB CLASS_NAMES_SQLITE";
    71        run;quit;
    NOTE: 2 observations were read from "SQLITE.class_names_sqlite"
    NOTE: Procedure print step took :
          real time : 0.031
          cpu time  : 0.031



    4                                                                                                                         Altair SLC

    NOTE: Libref SQLITE has been deassigned.
    72
    73        libname sqlite clear;
    74
    75        run;quit;
    ERROR: Error printed on page 1

    NOTE: Submitted statements took :
          real time : 0.602
          cpu time  : 0.296

    /*___              _                 _
     ( _ )    _____  _| |_ ___ _ __  ___(_) ___  _ __  ___
     / _ \   / _ \ \/ / __/ _ \ `_ \/ __| |/ _ \| `_ \/ __|
    | (_) | |  __/>  <| ||  __/ | | \__ \ | (_) | | | \__ \
     \___/   \___/_/\_\\__\___|_| |_|___/_|\___/|_| |_|___/

    */


    /**************************************************************************************************************************************/
    /*   NOTE SOME OF THESE FUNCTION ARE ONLY AVAILABLE ON THE SQLITE COMMAND LINE (IE MODE)                                              */
    /*               name          define_free    json_group_array      math_ceil           radians            stddev       time_fmt_iso  */
    /*                 ->              degrees   json_group_object       math_cos            random        stddev_pop      time_fmt_time  */
    /*                ->>           dense_rank         json_insert      math_cosh        randomblob       stddev_samp           time_get  */
    /*                abs           difference         json_object   math_degrees              rank             stdev       time_get_day  */
    /*               acos         dlevenshtein          json_patch       math_exp          readfile         strfilter      time_get_hour  */
    /*              acosh                dur_h         json_pretty     math_floor            regexp          strftime   time_get_isoweek  */
    /*                age                dur_m          json_quote        math_ln    regexp_capture        string_agg   time_get_isoyear  */
    /*               asin               dur_ms         json_remove       math_log       regexp_like            strpos    time_get_minute  */
    /*              asinh               dur_ns        json_replace     math_log10    regexp_replace            substr     time_get_month  */
    /*               atan                dur_s            json_set      math_log2     regexp_substr         substring      time_get_nano  */
    /*              atan2               dur_us           json_type       math_mod            repeat           subtype    time_get_second  */
    /*              atanh        edit_distance          json_valid        math_pi           replace               sum   time_get_weekday  */
    /*               atn2               encode               jsonb       math_pow         replicate           symlink      time_get_year  */
    /*                avg                 eval         jsonb_array   math_radians           reverse               tan   time_get_yearday  */
    /*         bit_length                  exp       jsonb_extract     math_round             right              tanh         time_micro  */
    /*             blake3        fileio_append   jsonb_group_array       math_sin          rightstr      text_bitsize         time_milli  */
    /*               bm25         fileio_mkdir  jsonb_group_object      math_sinh             round     text_casefold          time_nano  */
    /*              btrim          fileio_mode        jsonb_insert      math_sqrt        row_number       text_concat           time_now  */
    /*           casefold          fileio_read        jsonb_object       math_tan              rpad     text_contains         time_parse  */
    /*         caverphone       fileio_symlink         jsonb_patch      math_tanh          rsoundex        text_count         time_round  */
    /*               ceil         fileio_write        jsonb_remove     math_trunc        rtreecheck   text_has_prefix         time_since  */
    /*            ceiling          first_value       jsonb_replace            max        rtreedepth   text_has_suffix           time_sub  */
    /*            changes                floor           jsonb_set            md5         rtreenode        text_index      time_to_micro  */
    /*               char               format           julianday         median             rtrim         text_join      time_to_milli  */
    /*        char_length       fts3_tokenizer                 lag            min       script_code   text_last_index       time_to_nano  */
    /*   character_length                 fts5   last_insert_rowid          mkdir              sha1         text_left       time_to_unix  */
    /*          charindex      fts5_get_locale          last_value            mod            sha256       text_length         time_trunc  */
    /*           coalesce          fts5_locale                lead          .mode            sha384         text_like          time_unix  */
    /*             concat       fts5_source_id                left          nlike            sha512        text_lower         time_until  */
    /*          concat_ws          fuzzy_caver             leftstr         nlower              sign         text_lpad           timediff  */
    /*                cos         fuzzy_damlev              length            now               sin        text_ltrim       to_timestamp  */
    /*               cosh       fuzzy_editdist         levenshtein      nth_value              sinh       text_repeat              total  */
    /*                cot        fuzzy_hamming                like          ntile           snippet      text_replace      total_changes  */
    /*               coth        fuzzy_jarowin          likelihood         nullif           soundex      text_reverse          translate  */
    /*              count          fuzzy_leven              likely         nupper        split_part        text_right           translit  */
    /*      crypto_blake3        fuzzy_osadist                  ln   octet_length    sqlean_version         text_rpad               trim  */
    /*      crypto_decode       fuzzy_phonetic      load_extension        offsets        sqlite_log        text_rtrim              trunc  */
    /*      crypto_encode       fuzzy_rsoundex                 log       optimize  sqlite_source_id         text_size             typeof  */
    /*         crypto_md5         fuzzy_script               log10   osa_distance    sqlite_version        text_slice           unaccent  */
    /*        crypto_sha1        fuzzy_soundex                log2           padc              sqrt        text_split           undefine  */
    /*      crypto_sha256       fuzzy_translit               lower           padl            square    text_substring              unhex  */
    /*      crypto_sha384      gen_random_uuid      lower_quartile           padr       starts_with        text_title            unicode  */
    /*      crypto_sha512                 glob                lpad   percent_rank      stats_median    text_translate    unicode_version  */
    /*          cume_dist         group_concat              lsmode     percentile         stats_p25         text_trim          unixepoch  */
    /*       current_date              hamming               ltrim  percentile_25         stats_p75        text_upper           unlikely  */
    /*       current_time                  hex           make_date  percentile_75         stats_p90              time              upper  */
    /*  current_timestamp            highlight      make_timestamp  percentile_90         stats_p95          time_add     upper_quartile  */
    /*               date               ifnull               match  percentile_95         stats_p99     time_add_date              uuid4  */
    /*           date_add                  iif           matchinfo  percentile_99        stats_perc        time_after              uuid7  */
    /*          date_part                instr           math_acos  phonetic_hash      stats_stddev       time_before         uuid_blob   */
    /*         date_trunc         jaro_winkler          math_acosh             pi  stats_stddev_pop      time_compare          uuid_str   */
    /*           datetime                 json           math_asin            pow stats_stddev_samp         time_date           var_pop   */
    /*             decode           json_array          math_asinh          power         stats_var        time_equal          var_samp   */
    /*             define    json_array_length           math_atan         printf     stats_var_pop     time_fmt_date          variance   */
    /*       define_cache  json_error_position          math_atan2         proper    stats_var_samp time_fmt_datetime         writefile   */
    /*       json_extract           math_atanh          quote                                                                  zeroblob   */
    /**************************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
