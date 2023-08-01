# utl-rename-and-cast-char-to-numeric-using-do-over-arrays-in-natve-and-mysql-wps-r-python
    %let pgm=utl-rename-and-cast-char-to-numeric-using-do-over-arrays-in-natve-and-mysql-wps-r-python;

    Rename and cast char to numeric using do over arrays in native and mysql wps r python

    https://github.com/rogerjdeangelis/utl-rename-and-cast-char-to-numeric-using-do-over-arrays-in-natve-and-mysql-wps-r-python

       rename and change character columns to numeric

          MySQL code
             create
               table want as
             select
               sepallength + 0 as s_l /* adding 0 converts char '23' to numeric 33
              ,sepalwidth  + 0 as s_w
              ,petallength + 0 as p_l
              ,petalwidth  + 0 as p_w
             rom
              have

       SOLUTIONS

           1  wps datastep
           2  wps mysql
           3  wps sql
           4  native r
              https://stackoverflow.com/questions/76789278/mutate-multiple-columns-with-different-functions-using-a-metadata-tibble
              https://stackoverflow.com/users/10068985/darren-tsai
           5  wps python mysql
                          (seems faster and mor robust than R
           6  wps r mysql (could not  create table table1 as select column from table2)
                          (so I exported tabl2 to r dataframe mad the changes and imported to mysql)


    libname sd1 "d:/sd1";
    options validvarname=upcase;
    data sd1.have;informat
    SPECIES $10.
    SEPALLENGTH
    SEPALWIDTH
    PETALLENGTH
    PETALWIDTH $2.
    ;input
    SPECIES SEPALLENGTH SEPALWIDTH PETALLENGTH PETALWIDTH;
    cards4;
    Setosa 50 33 14 2
    Setosa 46 34 14 3
    Setosa 46 36 10 2
    Setosa 51 33 17 5
    Setosa 55 35 13 2
    Versicolor 65 28 46 15
    Versicolor 62 22 45 15
    Versicolor 59 32 48 18
    Versicolor 61 30 46 14
    Versicolor 60 27 51 16
    Virginica 64 28 56 22
    Virginica 67 31 56 24
    Virginica 63 28 51 15
    Virginica 69 31 51 23
    Virginica 65 30 52 20
    Virginica 65 30 55 18
    ;;;;
    run;quit;



    /*---- create table have in mysql sakila database                        ----*/
    %utl_submit_wps64x("

    libname mysqllib mysql user='root' password='xxxxxxxx' database='sakila' port=3306;

    libname sd1 'd:/sd1';

    proc sql;
      drop table mysqllib.have;
    ;quit;

    data mysqllib.have;
      set sd1.have;
    run;quit;

    Iluv2sk!!ksvulI

    libname mysqllib clear;

    /*                   _
    (_)_ __  _ __  _   _| |_ ___
    | | `_ \| `_ \| | | | __/ __|
    | | | | | |_) | |_| | |_\__ \
    |_|_| |_| .__/ \__,_|\__|___/
            |_|
    */

    /**************************************************************************************************************************/
    /* __      ___ __  ___                                         |                                                          */
    /* \ \ /\ / / `_ \/ __|                                        |                                                          */
    /*  \ V  V /| |_) \__ \                                        |                                                          */
    /*   \_/\_/ | .__/|___/                                        |                                                          */
    /*          |_|                                                |                                                          */
    /*                                                             |                                                          */
    /* Rename and convert to numeric                               |                                                          */
    /*                                                             |                                                          */
    /* INPUT                                                       |    WPS WANT DATASET OUTPUT                               */
    /*                                                             |                                                          */
    /* Middle Observation(8) table = sd1.have - Obs 16             |    Middle Observation(8) table = sd1.want - Obs          */
    /*                                                             |                                                          */
    /* Variable             Type   Value                           |    Variable        Type   Value                          */
    /*                                                             |                                                          */
    /* SPECIES               C10   Versicolor                      |    SPECIES          C10   Versicolor                     */
    /* SEPALLENGTH           C2    59                              |    S_L              N8    59                             */
    /* SEPALWIDTH            C2    32                              |    S_W              N8    32                             */
    /* PETALLENGTH           C2    48                              |    P_L              N8    48                             */
    /* PETALWIDTH            C2    18                              |    P_W              N8    18                             */
    /*                                                             |                                                          */
    /*                            _                                |                                                          */
    /*  _ __ ___  _   _ ___  __ _| |                               |                                                          */
    /* | `_ ` _ \| | | / __|/ _` | |                               |                                                          */
    /* | | | | | | |_| \__ \ (_| | |                               |                                                          */
    /* |_| |_| |_|\__, |___/\__, |_|                               |                                                          */
    /*            |___/        |_|                                 |                                                          */
    /*                                                             |   MYSQL WANT TABLE                                       */
    /*  MYSQL meta data nformation_schema.columns                  |                                                          */
    /*                                                             |                                                          */
    /*  COLUMN_NAME COLUMN_TYPE                                    |    COLUMN_NAME   COLUMN_TYPE                             */
    /*                          select                             |                                                          */
    /*                            column_name                      |                                                          */
    /*  PETALLENGTH varchar(2)   ,column_type                      |    P_L           double                                  */
    /*  PETALWIDTH  varchar(2)  from                               |    P_W           double                                  */
    /*  SEPALLENGTH varchar(2)    information_schema.columns       |    S_L           double                                  */
    /*  SEPALWIDTH  varchar(2)  where                              |    S_W           double                                  */
    /*  SPECIES     varchar(10)   table_name=`HAVE`                |    SPECIES       varchar(10)                             */
    /*                                                             |                                                          */
    /**************************************************************************************************************************/

    /*                            _       _            _
    / | __      ___ __  ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    | | \ \ /\ / / `_ \/ __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
    | |  \ V  V /| |_) \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |_|   \_/\_/ | .__/|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                 |_|                                          |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";

    data sd1.want;

       set sd1.have;
       array chr $2  sepallength --petalwidth;
       array num 8  s_l s_w p_l p_w;

       do over chr;
          num = input(chr,best.);
       end;

       drop sepallength -- petalwidth;

    run;quit;
    ');

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.WANT (S_L=P_W are now numeric)                                                                                     */
    /*                                                                                                                        */
    /*        Variables in Creation Order                                                                                     */
    /*                                                                                                                        */
    /* #    Variable    Type    Len    Format                                                                                 */
    /*                                                                                                                        */
    /* 1    SPECIES     Char     10    $10.                                                                                   */
    /* 2    S_L         Num       8                                                                                           */
    /* 3    S_W         Num       8                                                                                           */
    /* 4    P_L         Num       8                                                                                           */
    /* 5    P_W         Num       8                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*___                                                   _
    |___ \  __      ___ __  ___   _ __ ___  _   _ ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `_ ` _ \| | | / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | | | | | | |_| \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_| |_| |_|\__, |___/\__, |_|
                     |_|                    |___/        |_|
    */

    /*---- all processing done inside mysql database sakila                     ----*/
    /*---- used both libname and pass-through to provide examples               ----*/
    /*---- no need for separate access product                                  ----*/

    %utl_submit_wps64x("

    libname mysqllib mysql user='root' password='xxxxxxxx' database='sakila' port=3306;

    /*---- in case you rerun                                                    ----*/
    proc datasets lib=mysqllib nolist nodetails;
      delete want;
    run;quit;

    data mysqllib.want; /*---- create want table in database sakila            ----*/

       /*---- we are reading the have table in mysql sakila databse             ----*/
       set mysqllib.have;

       array chr $2  sepallength --petalwidth;
       array num 8  s_l s_w p_l p_w;

       do over chr;
          num = input(chr,best.);
       end;

       drop sepallength --petalwidth;

    run;quit;

    /*---- lets check example                                                   ----*/

    proc sql;
       connect to mysql (user='root' password='xxxxxxxx' database='sakila' port=3306);
     create
       table havMta as
     select column_name, column_type
       from connection to mysql
         (select column_name, column_type  from information_schema.columns where table_name=`WANT`);
        disconnect from mysql;

     quit;

    proc print data=havMta;
    run;quit;

    ");

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  MYSQL WANT TABLE                                                                                                      */
    /*                                                                                                                        */
    /*   COLUMN_NAME   COLUMN_TYPE                                                                                            */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*   P_L           double                                                                                                 */
    /*   P_W           double                                                                                                 */
    /*   S_L           double                                                                                                 */
    /*   S_W           double                                                                                                 */
    /*   SPECIES       varchar(10)                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                  _
    |___ /  __      ___ __  ___   ___  __ _| |
      |_ \  \ \ /\ / / `_ \/ __| / __|/ _` | |
     ___) |  \ V  V /| |_) \__ \ \__ \ (_| | |
    |____/    \_/\_/ | .__/|___/ |___/\__, |_|
                     |_|                 |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_wpsbeginx;
    parmcards4;
     options validvarname=any sasautos=("c:/otowps" sasautos);

     libname sd1 "d:/sd1";

     %array(_chr, values=petallength petalwidth sepallength sepalwidth);
     %array(_num, values=p_l p_w s_l s_w);

     proc sql;
       create
          table sd1.want as
       select
          species
         ,%do_over(_chr _num, phrase=%str(
            input(?_chr,best.) as ?_num), between=comma)
       from
          sd1.have
    ;quit;

    proc contents data=sd1.want;
    run;quit;

    /*----  CLEANUP                                                          ----*/
    %arraydelete(_chr);
    %arraydelete(_num);

    ;;;;
    %utl_wpsendx;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  WPS DATASET WANT                                                                                                      */
    /*                                                                                                                        */
    /*  Alphabetic List of Variables and Attributes                                                                           */
    /*                                                                                                                        */
    /*  Variable    Type             Len                                                                                      */
    /* _____________________________________________                                                                          */
    /*  SPECIES     Char              10                                                                                      */
    /*  p_l         Num                8                                                                                      */
    /*  p_w         Num                8                                                                                      */
    /*  s_l         Num                8                                                                                      */
    /*  s_w         Num                8                                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                 _   _
    | || |    _ __   __ _| |_(_)_   _____   _ __
    | || |_  | `_ \ / _` | __| \ \ / / _ \ | `__|
    |__   _| | | | | (_| | |_| |\ V /  __/ | |
       |_|   |_| |_|\__,_|\__|_| \_/ \___| |_|

    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(tidyverse);
    meta <- tibble(
        name = colnames(have),
        rename_as = c("s_l", "s_w", "p_l", "p_w", "spec"),
        format_as = c(rep("as.numeric", 4), "as.character");
    );
    meta;
    want<-have %>%
      mutate(across(everything(), ~ do.call(meta$format_as[meta$name == cur_column()], list(.x)),
                    .names = "{meta$rename_as[meta$name == .col]}"),
                    .keep = "unused");
    str(want);
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    proc contents data=sd1.want;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   R The WPS System                                                                                                     */
    /*                                                                                                                        */
    /* # A tibble: 5 x 3                                                                                                      */
    /*   name        rename_as format_as                                                                                      */
    /*   <chr>       <chr>     <chr>                                                                                          */
    /* 1 SPECIES     s_l       as.numeric                                                                                     */
    /* 2 SEPALLENGTH s_w       as.numeric                                                                                     */
    /* 3 SEPALWIDTH  p_l       as.numeric                                                                                     */
    /* 4 PETALLENGTH p_w       as.numeric                                                                                     */
    /* 5 PETALWIDTH  spec      as.character                                                                                   */
    /*                                                                                                                        */
    /* 'data.frame':        16 obs. of  5 variables:                                                                                 */
    /*  $ s_l : num  NA NA NA NA NA NA NA NA NA NA ...                                                                        */
    /*  $ s_w : num  50 46 46 51 55 65 62 59 61 60 ...                                                                        */
    /*  $ p_l : num  33 34 36 33 35 28 22 32 30 27 ...                                                                        */
    /*  $ p_w : num  14 14 10 17 13 46 45 48 46 51 ...                                                                        */
    /*  $ spec: chr  "2" "3" "2" "5" ...                                                                                      */
    /*                                                                                                                        */
    /*  WPS                                                                                                                   */
    /*                                                                                                                        */
    /*      Alphabetic List of Variables and Attributes                                                                       */
    /*                                                                                                                        */
    /*  Number    Variable    Type             Len             Pos                                                            */
    /*  __________________________________________________________                                                            */
    /*       3    P_L         Num                8              16                                                            */
    /*       4    P_W         Num                8              24                                                            */
    /*       5    SPEC        Char               2              32                                                            */
    /*       1    S_L         Num                8               0                                                            */
    /*       2    S_W         Num                8               8                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                      _   _                                            _
    | ___|   __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __   _ __ ___  _   _ ___  __ _| |
    |___ \   \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \ | `_ ` _ \| | | / __|/ _` | |
     ___) |   \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | || | | | | | |_| \__ \ (_| | |
    |____/     \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_||_| |_| |_|\__, |___/\__, |_|
                      |_|         |_|    |___/                                  |___/        |_|
    */

    %utl_submit_wps64x('
    proc python;
    submit;
    import pandas as pd;
    import mysql.connector;
    dataBase = mysql.connector.connect(host = "localhost",user = "root",passwd = "xxxxxxxx",database = "sakila" );
    cursorObject = dataBase.cursor();
    cursor = dataBase.cursor();
    cursor.execute("drop table if exists want");
    cursor.execute("
     create
        table want as
     select
        sepallength + 0 as s_l
       ,sepalwidth  + 0 as s_w
       ,petallength + 0 as p_l
       ,petalwidth  + 0 as p_w
     from
       have ");
    meta = pd.read_sql(
    "select column_name, column_type from information_schema.columns where table_name=`WANT`", con=dataBase);
    print(meta);
    dataBase.close();
    endsubmit;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* The PYTHON Procedure                                                                                                   */
    /*                                                                                                                        */
    /*  MYSQL TABLE WANT                                                                                                      */
    /*                                                                                                                        */
    /*  COLUMN_NAME COLUMN_TYPE                                                                                               */
    /*                                                                                                                        */
    /*           p_l   b'double'                                                                                              */
    /*           p_w   b'double'                                                                                              */
    /*           s_l   b'double'                                                                                              */
    /*           s_w   b'double'                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                                     _
    __      ___ __  ___   _ __   _ __ ___  _   _ ___  __ _| |
    \ \ /\ / / `_ \/ __| | `__| | `_ ` _ \| | | / __|/ _` | |
     \ V  V /| |_) \__ \ | |    | | | | | | |_| \__ \ (_| | |
      \_/\_/ | .__/|___/ |_|    |_| |_| |_|\__, |___/\__, |_|
             |_|                           |___/        |_|
    */

    %utl_submit_wps64x('
    proc r;
    submit;
     library(RMariaDB) ;
     library(DBI);
     library(dplyr);
     conn <- DBI::dbConnect(drv=RMariaDB::MariaDB(), user="root", password="xxxxxxxx",
        host="localhost", port=3306, dbname="sakila");
     dbExecute(conn, "drop table if exists want");
     want <- dbSendQuery(conn,
         "select
            sepallength + 0 as s_l
           ,sepalwidth  + 0 as s_w
           ,petallength + 0 as p_l
           ,petalwidth  + 0 as p_w
         from
           have");
     dw<-  dbFetch(want);
     rc <- dbWriteTable(conn, "want", dw, append=TRUE, temporary = FALSE);
     chk <- dbSendQuery(conn,
          "select column_name, column_type from information_schema.columns where table_name=`WANT`");
     dwc<-  dbFetch(chk);
     dwc;
     dbDisconnect(conn);
     endsubmit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* PROC R Procedure                                                                                                       */
    /*                                                                                                                        */
    /*  MYSQL TABLE WANT                                                                                                      */
    /*                                                                                                                        */
    /*  COLUMN_NAME COLUMN_TYPE                                                                                               */
    /*                                                                                                                        */
    /*           p_l   b'double'                                                                                              */
    /*           p_w   b'double'                                                                                              */
    /*           s_l   b'double'                                                                                              */
    /*           s_w   b'double'                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                         _
     _ __ ___  _   _ ___  __ _| |  _ __ ___ _ __   ___  ___
    | `_ ` _ \| | | / __|/ _` | | | `__/ _ \ `_ \ / _ \/ __|
    | | | | | | |_| \__ \ (_| | | | | |  __/ |_) | (_) \__ \
    |_| |_| |_|\__, |___/\__, |_| |_|  \___| .__/ \___/|___/
               |___/        |_|            |_|
    */

    https://github.com/rogerjdeangelis/mySQL-uml-modeling-to-create-entity-diagrams-for-sas-datasets
    https://github.com/rogerjdeangelis/utl-PASSTHRU-to-mysql-and-select-rows-based-on-a-SAS-dataset-without-loading-the-SAS-daatset-into
    https://github.com/rogerjdeangelis/utl-accessing-a-mysql-database-using-R-without-the-sas-access-product
    https://github.com/rogerjdeangelis/utl-mysql-queries-without-sas-using-r-python-and-wps
    https://github.com/rogerjdeangelis/utl_examples_SAS_mysql_interface_on_power_workstation
    https://github.com/rogerjdeangelis/utl_explicit_pass_through_to_mysql_to_subset_a_table_using_macro_variable_dates
    https://github.com/rogerjdeangelis/utl_exporting_a_sas_single_60k_string_to_mysql_and_reading_it_back_into_two_30k_strings
    https://github.com/rogerjdeangelis/utl_extract_from_mySQL_add_index
    https://github.com/rogerjdeangelis/utl_sql_update_master_using_a_transaction_table_mysql_database
    https://github.com/rogerjdeangelis/utl_with_a_press_of_a_function_key_convert_the_highlighted_dataset_to_a_mysql_database_table

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
Rename and cast char to numeric using do over arrays in natve and mysql wps r python 
