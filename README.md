# utl_sql_insert_if_in_another_table
If records are missing in one table but exist in another then insert the missing records

    ```  If age=16 is missing in className table but exists in classNameAge16 then insert it into class  ```
    ```    ```
    ```    If age=16 is missing in class table but exists in classNameAge16 then insert it into class  ```
    ```    ```
    ```    WORKING CODE  ```
    ```    ============  ```
    ```    ```
    ```       DOSUBL  ```
    ```    ```
    ```         * macro variable classNameNo16=0 if age=16 in not className dataset;  ```
    ```         select count(*) into :classNameNo16  trimmed from className      where age=16;  ```
    ```    ```
    ```         * macro variable classNameAge16=1 if age=16 in classNameAge16 dataset;  ```
    ```         select count(*) into :classNameAge16 trimmed from classNameAge16 where age=16;  ```
    ```    ```
    ```       MAINLINE SQL  ```
    ```    ```
    ```         insert into className  ```
    ```         select *  ```
    ```         from classNameAge16  ```
    ```         where symget('classNameNo16')  = '0' and  /* not in classNameNo16 dataset */  ```
    ```               symget('classNameAge16') = '1' and  /* in classNameAge16  dataset */  ```
    ```               age=16  ```
    ```    ```
    ```  Unfortunately SQL does not have the concept of a null table so  ```
    ```  I keep an empty dataset in my SASUSER library for solutions like the one below.  ```
    ```    ```
    ```  see  ```
    ```  https://goo.gl/PHv565  ```
    ```  https://communities.sas.com/t5/Base-SAS-Programming/how-to-check-  ```
    ```  if-a-variable-not-exists-in-a-dataset-first-then/m-p/394490#M95073  ```
    ```    ```
    ```  HAVE  ```
    ```  ====  ```
    ```    ```
    ```  className does not have age=16 but classNameAge16 does  ```
    ```    ```
    ```  WORK.CLASSNAMENO16 total obs=4  ```
    ```    ```
    ```     Obs     NAME      AGE  ```
    ```    ```
    ```      1     Robert      12  ```
    ```      2     Ronald      15  ```
    ```      3     Thomas      11  ```
    ```      4     William     15  ```
    ```    ```
    ```    ```
    ```    WORK.CLASSNAMEAGE16 total obs=5  ```
    ```    ```
    ```     Obs     NAME      AGE  ```
    ```    ```
    ```      1     Philip      16  ```
    ```      2     Robert      12  ```
    ```      3     Ronald      15  ```
    ```      4     Thomas      11  ```
    ```      5     William     15  ```
    ```    ```
    ```  WANT  ```
    ```  ====  ```
    ```    ```
    ```    classNameNo16 total obs=5  ```
    ```    ```
    ```    Obs     NAME      AGE  ```
    ```    ```
    ```     1     Robert      12  ```
    ```     2     Ronald      15  ```
    ```     3     Thomas      11  ```
    ```     4     William     15  ```
    ```    ```
    ```     5     Philip      16   * added this record  ```
    ```    ```
    ```  *                _              _       _  ```
    ```   _ __ ___   __ _| | _____    __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  %symdel classNameNo16 classNameAge16 / nowarn;  ```
    ```  proc datasets lib=work;  ```
    ```  delete class:;  ```
    ```  run;quit;  ```
    ```    ```
    ```  data classNameNo16;  ```
    ```    set sashelp.class(firstobs=15 keep=name age where=(age ne 16));  ```
    ```  run;quit;  ```
    ```    ```
    ```  data classNameAge16;  ```
    ```    set sashelp.class(firstobs=15 keep=name age);  ```
    ```  run;quit;  ```
    ```    ```
    ```  data sasuser.empty;  ```
    ```    retain empty .;  ```
    ```  run;quit;  ```
    ```    ```
    ```  *          _       _   _  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  ```
    ```    ```
    ```  ;  ```
    ```  proc sql;  ```
    ```    select  ```
    ```      %sysfunc(dosubl('  ```
    ```       proc sql ;  ```
    ```         select count(*) into :classNameNo16  trimmed from classNameNo16  where age=16;  ```
    ```         select count(*) into :classNameAge16 trimmed from classNameAge16 where age=16;  ```
    ```       ;quit;  ```
    ```      '))  as rc  ```
    ```    from sasuser.empty;  ```
    ```    %put &=classNameNo16 &=classNameAge16;  ```
    ```    insert  ```
    ```       into classNameNo16  ```
    ```    select  ```
    ```       *  ```
    ```    from  ```
    ```      classNameAge16  ```
    ```    where  ```
    ```      symget('classNameNo16')  = '0' and  ```
    ```      symget('classNameAge16') = '1' and  ```
    ```      age=16  ```
    ```  ;quit;  ```
    ```    ```
    ```  1124  proc sql;  ```
    ```  1125    select  ```
    ```  1126      %sysfunc(dosubl('  ```
    ```  NOTE: PROCEDURE SQL used (Total process time):  ```
    ```        real time           0.04 seconds  ```
    ```        cpu time            0.03 seconds  ```
    ```    ```
    ```    ```
    ```  1127       proc sql ;  ```
    ```  1128         select count(*) into :classNameNo16  trimmed from classNameNo16  where age=16;  ```
    ```  1129         select count(*) into :classNameAge16 trimmed from classNameAge16 where age=16;  ```
    ```  1130       ;quit;  ```
    ```  1131      '))  as rc  ```
    ```  1132    from sasuser.empty;  ```
    ```  1133    %put &=classNameNo16 &=classNameAge16;  ```
    ```  CLASSNAMENO16=0 CLASSNAMEAGE16=1  ```
    ```  1134    insert  ```
    ```  1135       into classNameNo16  ```
    ```  1136    select  ```
    ```  1137       *  ```
    ```  1138    from  ```
    ```  1139      classNameAge16  ```
    ```  1140    where  ```
    ```  1141      symget('classNameNo16')  = '0' and  ```
    ```  1142      symget('classNameAge16') = '1' and  ```
    ```  1143      age=16  ```
    ```  1144  ;  ```
    ```    ```
    ```    ```
