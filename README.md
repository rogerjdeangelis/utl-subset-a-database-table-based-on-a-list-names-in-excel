# utl-subset-a-database-table-based-on-a-list-names-in-excel
Subset a database table based on a list names in excel.
    Subset a database table based on a list names in excel

    github
    https://tinyurl.com/yarmhrz2
    https://github.com/rogerjdeangelis/utl-subset-a-database-table-based-on-a-list-names-in-excel


    INPUT (excel workbook with sheet have aand SASHELP.CLASS)
    ======================================

     d:/xls/strcomma.xlsx

       +-----------------------------------+
       |                A                  |
       +-----------------------------------+
       |                X                  |
       +-----------------------------------+
       | Alice, Alred, William             |
       +-----------------------------------+

     [HAVE]

     AND SASHELP.CLASS

      SASHELP.CLASS total obs=19

       NAME       SEX    AGE    HEIGHT    WEIGHT

       Alfred      M      14     69.0      112.5
       Alice       F      13     56.5       84.0
       Barbara     F      13     65.3       98.0
       Carol       F      14     62.8      102.5
       Henry       M      14     63.5      102.5
       ....


    EXAMPLE OUTPUT (subset sashelp.class)
    --------------------------------------

     WORK.WANT total obs=3

      Obs     NAME      SEX    AGE    HEIGHT    WEIGHT

       1     Alfred      M      14     69.0      112.5
       2     Alice       F      13     56.5       84.0
       3     William     M      15     66.5      112.0


    PROCESS
    =======

    libname xel "d:/xls/strcomma.xlsx";

    %symdel x /nowarn;
    data want;

      * to simplify quoting I use the hex representation for double quote and comma;
      if _n_=0 then do;
         %let rc=%sysfunc(dosubl('
            proc sql;
              select
                 compress(tranwrd(x,"2C"x,"222C22"x))
              into
                 :x trimmed
              from
                 xel.have
            ;quit;
         '));
      end;

      set sashelp.class;
      if name in ("&x.");

    run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    %utlfkil(d:/xls/strcomma.xlsx); * delete if exist;

    libname xel "d:/xls/strcomma.xlsx";

    data xel.have;
      x='Alice, Alfred, William';
    run;quit;

    libname xel clear;


