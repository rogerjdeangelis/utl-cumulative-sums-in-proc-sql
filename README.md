# utl-cumulative-sums-in-proc-sql
Cumulative sums in proc sql
    Cumulative sums in proc sql

    github
    https://tinyurl.com/y9q66y3a
    https://github.com/rogerjdeangelis/utl-cumulative-sums-in-proc-sql

    for those who require a solution in relational databases.

    SAS Forum
    https://communities.sas.com/t5/SAS-Studio/cumulative-sum-SQL/m-p/643024

    Solution by
    Bartosz Jablonski
    yabwon@gmail.com
    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
     input Employee $ Salary;
    cards;
    emp1 1
    emp3 7
    emp4 4
    ;;;;
    run;quit;

    WORK.WANT total obs=3
                            CUMULATIVE_
      EMPLOYEE    SALARY        SUM

        emp1         1            1
        emp3         7            8
        emp4         4           12

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WORK.WANT total obs=3

                            CUMULATIVE_
      EMPLOYEE    SALARY        SUM

        emp1         1            1
        emp3         7            8     1+7 =  8
        emp4         4           12     8+4 = 12

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;


    proc sql;
      create
         table want as
      select
         a.*
         ,sum(b.salary) as cumulative_sum
      from
          have a inner join have b
      on
          b.employee<=a.employee    /* key to eliminating double counting  */
      group
          by a.employee,a.salary
    ;quit;



