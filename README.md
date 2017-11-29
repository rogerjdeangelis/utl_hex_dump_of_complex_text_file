# utl_hex_dump_of_complex_text_file
The tool utlrulr(hex ruler) breaks the file into 100 byte chunks and displays the all the hex codes.  for latest hex ruler tool see (also on the end) https://github.com/rogerjdeangelis/utl_hex_ruler  You need to spend time with utlrulr tool to understand the both he switching from '20'x to '09'x and the end of record hex code.

    Hex dump of complex text file

    Post title Problem importing a txt-file or hex dump of text file

    The tool utlrulr(hex ruler) breaks the file into 100 byte chunks
    and displays the all the hex codes.

    for latest hex ruler tool see (also on the end)
    https://github.com/rogerjdeangelis/utl_hex_ruler

    You need to spend time with utlrulr tool to understand the both he
    switching from '20'x to '09'x and the end of record hex code.

    see
    https://goo.gl/nkuXuG
    https://communities.sas.com/t5/Base-SAS-Programming/Problem-importing-a-txt-file/m-p/416876


    INPUT
    =====

      Download the text file  from https://goo.gl/nkuXuG or

      https://goo.gl/ufQnXm
      https://github.com/rogerjdeangelis/utl_hex_dump_of_complex_text_file/blob/master/utl_hex_dump_of_complex_text_file.txt

      First two 100 byte chunks

      1.411895789 1.44330829 1.49552952 1.55391441 1.59009382 1.614501576 1.637530421 1.665363942 1.691646
      417 1.715162301 1.736882051 1.757064002 1.775726446 1.79324709 1.809712927 1.824861859 1.838941566 1
      ....

    PROCESS
    =======

       %utlrulr
          (
           uinflt =d:/txt/DataSample.txt,
           uprnlen =100,    /* Linesize for Dump */
           ulrecl  =100,    /* maximum record length */
           urecfm   =f,
           uobs = 100,       /* number of obs to dump */
           uotflt =d:/txt/DataSampleHex.txt
          );

    OUTPUT
    =====
       ASCII Flatfile Ruler & Hex
       utlrulr
       d:/txt/DataSample.txt
       d:/txt/DataSampleHex.txt


        --- Record Number ---  1   ---  Record Length ---- 100

       1.411895789 1.44330829 1.49552952 1.55391441 1.59009382 1.614501576 1.637530421 1.665363942 1.691646
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90...95...1
       3233333333323233333333232333333332323333333323233333333232333333333232333333333232333333333232333333
       1E41189578901E4433082901E4955295201E5539144101E5900938201E61450157601E63753042101E66536394201E691646
                  ^          ^
                  |          |
                 '20'x      '20'x

        --- Record Number ---  2   ---  Record Length ---- 100

       417 1.715162301 1.736882051 1.757064002 1.775726446 1.79324709 1.809712927 1.824861859 1.838941566 1
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90...95...1
       3332323333333332323333333332323333333332323333333332323333333323233333333323233333333323233333333323
       41701E71516230101E73688205101E75706400201E77572644601E7932470901E80971292701E82486185901E83894156601

       ......

        --- Record Number ---  8   ---  Record Length ---- 100

       2087892.2.124912464.2.128907698.2.132874505.2.136818091.2.14073502 2.144637454 2.148521644 2.1523984
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90...95...1
       3333333032333333333032333333333032333333333032333333333032333333332323333333332323333333332323333333
       208789292E12491246492E12890769892E13287450592E13681809192E1407350202E14463745402E14852164402E1523984
              ^           ^           ^           ^           ^          ^
              |           |           |           |           |          |
             '09'x       '09'x       '09'x       '09'x       '09'x      '20'x ** Switched from '09'x to '20'x


      %macro utlrulr
          (
           utitle=ASCII Flatfile Ruler & Hex,
           uobj  =utlrulr,
           uinflt =c:\dat\delete.dat,
           /*--------------------------------*\
           | Control                          |
           \*--------------------------------*/
           uprnlen =70,  /* Linesize for Dump */
           ulrecl  =32760,  /* maximum record length */
           urecfm   =,
           uobs = 3,        /* number of obs to dump */
           uchrtyp =ascii,  /* ascii or ebcdic */
           /*--------------------------------*\
           | Outputs                          |
           \*--------------------------------*/
           uotflt =c:\dat\delete.hex
          ) / des = "ASCII Flatfile Ruler & Hex Chars";
        /*----------------------------------------------*\
        | Description:                                   |
        |  This code does a hex dump of a flatfile       |
        |  IPO                                           |
        |  ===                                           |
        |   INPUTS    UINFLT                             |
        |   ======                                       |
        |   A flatfile like                              |
        |    A2  NC 199607 JAN96   37   16    43  12     |
        |    A1  SC 199601 JAN91   17   26    45  32     |
        |    A2  ND 199602 JAN92   32   36    46         |
        |    A3  VT 199603 JAN93   47   54    47  35     |
        |    A4  NY 199604 JAN94   35   55    48  62     |
        |    A5  CA 199605 JAN95   57   56    49         |
        |    A6  NC 199606 JAN96   36   57    89  39     |
        |                                                |
        |   PROCESS                                      |
        |   =======                                      |
        |    Create a ruler line for each                |
        |    record. Echo the ASCII text.                |
        |    Print two lines with the hex                |
        |    representation.                             |
        |                                                |
        |   OUTPUT   UOTFLT    hex dump                  |
        |   ======                                       |
        |   Sample output                                |
        |    --- Record Number ---- 1                    |
        |    --- Record Length ---- 50                   |
        |                                                |
        |   A2  NC 199607 JAN96   37                     |
        |   1...5....10...15...20...2                    |
        |   4322442333333244433222332                    |
        |   1200E301996070A1E96000370                    |
        |                                                |
        |     16    43  12        567                    |
        |   5...30...35...40...45...5                    |
        |   2233222233223322222222333                    |
        |   0016000043001200000000567                    |
        |                                                |
        |    --- Record Number ---  2                    |
        |    --- Record Length ---- 39                   |
        |                                                |
        |   A1  SC 199601 JAN91   17                     |
        |   1...5....10...15...20...2                    |
        |   4322542333333244433222332                    |
        |   11003301996010A1E91000170                    |
        |     26    45  32                               |
        |   5...30...35...                               |
        |   22332222332233                               |
        |   00260000450032                               |
        |   Limitations                                  |
        |   1. Only handles records up to 1000 bytes     |
        |   2. Is very slow                              |
        \*----------------------------------------------*/
        /*--------------------------------------------------------------*\
        |  TESTCASE                                                      |
        |   data _null_;                                                 |
        |      file "c:\utl\delete.tst";                                 |
        |      put "A2  NC 199607 JAN96   37   16    43  12        567"; |
        |      put "A1  SC 199601 JAN91   17   26    45  32";            |
        |      put "A2  ND 199602 JAN92   32   36    46  42";            |
        |      put "A3  VT 199603 JAN93   47   54    47  35";            |
        |      put "A4  NY 199604 JAN94   35   55    48  62        789"; |
        |      put "A5  CA 199605 JAN95   57   56    49  72";            |
        |      put "A6  NC 199606 JAN96   36   57    89  39";            |
        |   run;                                                         |
        |   %utlrulr                                                     |
        |      (                                                         |
        |       uprnlen=25,                                              |
        |       uobs=3,                                                  |
        |       uinflt=c:\utl\delete.tst,                                |
        |       uotflt=c:\utl\delete.hex                                 |
        |      );                                                        |
        \*--------------------------------------------------------------*/
         options ls=80 ps=63;run;
         title1 "&utitle.";
         title2 "&uobj.";
         title3 "&uinflt.";
         title4 "&uotflt.";
         data _null_; file "&uotflt" print; put; run;
         title1;
           data _NULL_;
              length col $5. out $1;
              array uchr{1000} $1  u1-u1000;
              array urul{1000} $1  r1-r1000;
              infile "&uinflt" missover end=done length=ln lrecl=&ulrecl ignoredoseof
              %if "&urecfm"  ne  "" %then %str( recfm=&urecfm );
              ;
              file   "&uotflt" mod;
              input (uchr{*}) ( $char1.);
              recnum + 1;
              put #1 @;
              put #2 @;
              put #3 @uk ' --- Record Number ---  ' recnum '  ---  Record Length ---- ' ln  @;
              put #4 @;
              /*--------------------------------*\
              | Build the Ruler                  |
              \*--------------------------------*/
              urul{1}='1';
              urul{2}='.';
              urul{3}='.';
              urul{4}='.';
              do ui = 5 to 995 by 5;
                 col  = compress( put ( ui, 4. ) !!'....' );
                 do uj = 1 to 5;
                    ul = ui + uj -1;
                    urul{ul} = substr( col, uj, 1 );
                 end;
              end;
              /*--------------------------------*\
              | Build Character & Hex Dump       |
              \*--------------------------------*/
              do ui = 1 to ln;
                 uk+1;
                 out = prxchange('s/[\x00-\x1F]/./', -1, uchr{ui});
                 out = right(prxchange('s/[\x7F-\xFF]/./', -1, out));
                 put #5 @uk out $char&uprnlen.. @;
                 put #6 @uk urul{ui} $1. @;
                 nibble1 = substr( put ( uchr{ui}, $hex2. ), 1, 1 );
                 put #7 @uk nibble1 @;
                 nibble2 = substr( put ( uchr{ui}, $hex2. ), 2, 1 );
                 put #8 @uk nibble2 @;
                 if mod( ui, &uprnlen ) eq 0 or  ( ln = ui )  then do;
                    put ;
                    uk=0;
                 end;
              end;
              put ///;
              if recnum eq &uobs then stop;
          run;
       %mend utlrulr;



