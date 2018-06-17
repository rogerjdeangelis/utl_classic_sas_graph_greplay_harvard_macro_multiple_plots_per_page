# utl_classic_sas_graph_greplay_harvard_macro_multiple_plots_per_page
Classic sas graph greplay Harvard macro multiple plots per page.    Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Classic sas graph greplay Harvard macro multiple plots per page

    Replay two graphs on one page vertically


    INPUT
    =====

    Catalog with a template and two separate bar charts


           Contents of Catalog WORK.TEMPCAT

      #    Name        Type          Description
      --------------------------------------------------------
      1    AGE         GRSEG         Bar chart of AGE
      2    SEX         GRSEG         Bar chart of AGE

      3    TEMPLATE    TEMPLATE      2 by 1 graphs in one page


      * code that creaed the above;
      proc catalog cat=work.tempcat;
        contents;
      run;quit;


    You can also use the SAS privided templates instead of the one above

      NOTE: Catalog members: SASHELP.TEMPLT

      NAME          DESCRIPTION

      H2            1 BOX LEFT, 1 BOX RIGHT
      H2S           1 BOX LEFT, 1 BOX RIGHT (WITH SPACE)
      H3            3 BOXES ACROSS (HORIZONTALLY)
      H3S           3 BOXES ACROSS (WITH SPACE)
      H4            4 BOXES ACROSS (HORIZONTALLY)
      H4S           4 BOXES ACROSS (WITH SPACE)
      L1R2          1 BOX LEFT, 2 BOXES RIGHT
      L1R2S         1 BOX LEFT, 2 BOXES RIGHT (WITH SPACE)
      L2R1          2 BOXES LEFT, 1 BOX RIGHT
      L2R1S         2 BOXES LEFT, 1 BOX RIGHT (WITH SPACE)
      L2R2          2 BOXES LEFT, 2 BOXES RIGHT
      L2R2S         2 BOXES LEFT, 2 BOXES RIGHT (WITH SPACE)
      U1D2          1 BOX UP, 2 BOXES DOWN
      U1D2S         1 BOX UP, 2 BOXES DOWN (WITH SPACE)
      U2D1          2 BOXES UP, 1 BOX DOWN
      U2D1S         2 BOXES UP, 1 BOX DOWN (WITH SPACE)
      V2            1 BOX UP, 1 BOX DOWN
      V2S           1 BOX UP, 1 BOX DOWN (WITH SPACE)
      V3            3 BOXES STACKED VERTICALLY
      V3S           3 BOXES STACKED VERTICALLY (WITH SPACE)
      WHOLE         ENTIRE SCREEN TEMPLATE

    PROCESS  (once you have creaed and loaded the catalog)
    =======================================================

      goptions display;
      proc greplay nofs igout=tempcat gout=tempcat tc=tempcat;
        template template;
        treplay 1:Sex
                2:Age
        ;
      run;quit;

    OUTPUT
    ======

    +-----------------------------------------------------------+
    |    WEIGHT Mean                                            |
    |                                                           |
    | 150 +                                             *****   |
    |     |                                             *****   |
    |     |                                             *****   |
    |     |                                   *****     *****   |
    |     |                                   *****     *****   |
    | 100 +                         *****     *****     *****   |
    |     |               *****     *****     *****     *****   |
    |     |               *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |  50 +     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     ----------------------------------------------------- |
    |            11.4      12.6      13.8      15.0      16.2   |
    |                                                           |
    |                           AGE Midpoint                    |
    |                                                           |
    | WEIGHT Mean                                               |
    |                                                           |
    | 150 +                                             *****   |
    |     |                                             *****   |
    |     |                                             *****   |
    |     |                                   *****     *****   |
    |     |                                   *****     *****   |
    | 100 +                         *****     *****     *****   |
    |     |               *****     *****     *****     *****   |
    |     |               *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |  50 +     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     |     *****     *****     *****     *****     *****   |
    |     ----------------------------------------------------- |
    |            11.4      12.6      13.8      15.0      16.2   |
    |                                                           |
    |                           AGE Midpoint                    |
    +-----------------------------------------------------------+


    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    * macro that can be used to create many templates;

    %macro utl_mulgrf(rows, cols) / des="multiple graphics on a page";

       %put Multiple graphics per page macro ;
       %put Sung-Il Cho - Harvard School of Public Health ;
       %put March 20, 1995 ;

      proc greplay igout=work.gseg tc=tempcat nofs;
      tdef template des="&rows by &cols graphs in one page"

      %do i=1 %to %eval(&rows);
        %do j=1 %to %eval(&cols);
           %eval(&j+(&i-1)*(&cols))/
         ulx=%eval(100*(&j-1)/(&cols))   uly=%eval(100*(&rows+1-&i)/(&rows))
         urx=%eval(100*(&j)  /(&cols))   ury=%eval(100*(&rows+1-&i)/(&rows))
         llx=%eval(100*(&j-1)/(&cols))   lly=%eval(100*(&rows-&i)  /(&rows))
         lrx=%eval(100*(&j)  /(&cols))   lry=%eval(100*(&rows-&i)  /(&rows))
        %end;
      %end;
      %str(;)
      template template;
      treplay
          %do i=1 %to %eval(&rows);
            %do j=1 %to %eval(&cols);
              %eval(&j+(&i-1)*&cols) : %eval(&j+(&i-1)*&cols)
            %end;
          %end;
      %str(;)
    run;quit;
    %mend utl_mulgrf;


    * just in case catelogs exist, also prevents numbered output graphs;

    proc catalog cat=gseg kill;
    run;quit ;

    proc catalog cat=tempcat kill;
    run;quit ;

    filename grfout "d:\png\two.png";
    goptions reset=all device=png target=png gsfname=grfout display htext=3;

    proc gchart data=sashelp.class gout=tempcat;
          /* chart for upper quadrant */
          vbar age   / type=mean sumvar=weight  name='Sex'
                      patternid=midpoint;
          run;
          /* chart for lower quadrant */
          vbar age  / type=mean sumvar=weight   name='Age'
                      patternid=midpoint;
          run;
     quit;

    /* two rows and one column - portrait like*/
    %utl_mulgrf(2,1);

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process
