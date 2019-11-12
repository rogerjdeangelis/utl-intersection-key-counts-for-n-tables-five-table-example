# utl-intersection-key-counts-for-n-tables-five-table-example
Intersection key counts for n tables five table example

    Intersection key counts for n tables five table example

    Given 5 datasets there are 31 possible intersections of keys (2**5-1).
    There are 25 pairings.

    github
    https://tinyurl.com/y2goyne5
    https://github.com/rogerjdeangelis/utl-intersection-key-counts-for-n-tables-five-table-example

    Ehnancements by                                                                                  
                                                                                                     
       Richard DeVenezia <rdevenezia@GMAIL.COM>                                                      
       Bart Jablonski <yabwon@GMAIL.COM>                                                             
                                                                                                     
    on end                                                                                           
                                                                                                     
    From:              
    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    %let tables=%sysevalf(2**5 - 1);

    data T1 T2 T3 T4 T5 ;
      call streaminit(4321);
      do I=1 to 30;
        if rand("uniform")>.6 then output T1;
        if rand("uniform")>.6 then output T2;
        if rand("uniform")>.6 then output T3;
        if rand("uniform")>.6 then output T4;
        if rand("uniform")>.6 then output T5;
      end;
    run;

                     TABLES

           T1    T2    T3     T4    T5

    Obs     I     I     I      I     I

      1     1     5     1      5     5
      2     2     8     3      8     6
      3     3     9     4      9     9
      4     4    10     5     16    11
      5     5    12     9     18    12
      6     6    15    12     19    13
      7     7    16    18     20    16
      8    10    20    21     22    18
      9    16    21    24     23    19
     10    19    25    25     27    21
     11    20    27    27     30    22
     12    22    28    28           26
     13    24    29    29           27
     14    27          30           29
     15    29
    *           _
     _ __ _   _| | ___  ___
    | '__| | | | |/ _ \/ __|
    | |  | |_| | |  __/\__ \
    |_|   \__,_|_|\___||___/
                                          | RULES (Key cobinations are only counted once)
    ;                                     |
                                          |
           T1    T2    T3     T4    T5    | 15 Times T1 intersects T1 perfectly 15 times 1=1,2=2...29-29
                                          |
    Obs     I     I     I      I     I    | 13 T2 intersects T2 perfectly 13 times 5=5,8=8...29-29
                                          |
      1     1     5     1      5     5    |  6 Times T1 intersects T2 5=5,10=10,16=16,20=20,27=27,29=29
      2     2     8     3      8     6    |
      3     3     9     4      9     9    |  3 T1 intersects T2 and T3 5=5,27=27 abd 29=29
      4     4    10     5     16    11    |
      5     5    12     9     18    12    |
      6     6    15    12     19    13    |
      7     7    16    18     20    16    |
      8    10    20    21     22    18    |
      9    16    21    24     23    19    |
     10    19    25    25     27    21    |
     11    20    27    27     30    22    |
     12    22    28    28           26    |
     13    24    29    29           27    |
     14    27          30           29    |
     15    29                             |


    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    Herea are the 31 possibilities

    WORK.BASE total obs=31

    Obs    BIN     MRG               CNT

     1    10000    T1                 15
     2    01000    T2                 13
     3    11000    T1 T2               6
     4    00100    T3                 14
     5    10100    T1 T3               7
     6    01100    T2 T3               8
     7    11100    T1 T2 T3            3
     8    00010    T4                 11
     9    10010    T1 T4               6
    10    01010    T2 T4               6
    11    11010    T1 T2 T4            4
    12    00110    T3 T4               5
    13    10110    T1 T3 T4            2
    14    01110    T2 T3 T4            3
    15    11110    T1 T2 T3 T4         2
    16    00001    T5                 14
    17    10001    T1 T5               7
    18    01001    T2 T5               7
    19    11001    T1 T2 T5            4
    20    00101    T3 T5               7
    21    10101    T1 T3 T5            3
    22    01101    T2 T3 T5            6
    23    11101    T1 T2 T3 T5         3
    24    00011    T4 T5               7
    25    10011    T1 T4 T5            5
    26    01011    T2 T4 T5            4
    27    11011    T1 T2 T4 T5         3
    28    00111    T3 T4 T5            4
    29    10111    T1 T3 T4 T5         2
    30    01111    T2 T3 T4 T5         3
    31    11111    T1 T2 T3 T4 T5      2


    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    %utlnopts;
    %symdel mrg merg bin/npwarn;
    data _null_;
      if _n_=0 then do; %let rc=%sysfunc(dosubl('
         proc datasets lib=work nolist;
           delete base;
         run;quit;
         '));
       end;
      length mrg $200;
      retain mrg;
      do bin=1 to 31;
         binstr=left(reverse(put(bin,binary5.)));
         do i=1 to 5;
           if substr(binstr,i,1) ne '0' then mrg=catx(" ",mrg,cats('T',i));
         end;
         put mrg=;
         call symputx("mrg",mrg);
         call symputx("bin",binstr);
         rc=dosubl('
            %array(merg,values=&mrg);
            proc datasets nolist;
               delete binOne;
            run;quit;
            data binOne;
              bin=symget("bin");
              length mrg $200;
              retain cnt flg 0;
              merge %do_over(merg,phrase=%str(?(in=?))) end=dne;
              by i;
              mrg=symget("mrg");
              if dne then flg=1;
              if %do_over(merg,phrase=?,between=and) then do;
                 cnt=cnt+1;
              end;
              if flg=1 then output;
              drop i flg;
            run;quit;
            proc append data=binOne base=base;
            run;quit;
         ');
         mrg="";
      end;
      stop;
    run;quit;
                                                                              
                                                                                                     
    *____  _      _                   _                                                              
    |  _ \(_) ___| |__   __ _ _ __ __| |                                                             
    | |_) | |/ __| '_ \ / _` | '__/ _` |                                                             
    |  _ <| | (__| | | | (_| | | | (_| |                                                             
    |_| \_\_|\___|_| |_|\__,_|_|  \__,_|                                                             
                                                                                                     
    ;                                                                                                
    Richard DeVenezia <rdevenezia@GMAIL.COM>                                                         
                                                                                                     
    Hi Roger:                                                                                        
                                                                                                     
    On the presumption of unique keys, the use of bit masks and binary anding                        
    can examine (test) and accumulate frequency counts of the merge flag                             
    permutations.                                                                                    
                                                                                                     
    Reversing the in= flag variable names with respect to the input data sets                        
    defines the bit ordering at compilation time, eliminating the need of a                          
    reverse() invocation.                                                                            
                                                                                                     
    data T1 T2 T3 T4 T5 ;                                                                            
      call streaminit(4321);                                                                         
      do I=1 to 30;                                                                                  
        if rand("uniform")>.6 then output T1;                                                        
        if rand("uniform")>.6 then output T2;                                                        
        if rand("uniform")>.6 then output T3;                                                        
        if rand("uniform")>.6 then output T4;                                                        
        if rand("uniform")>.6 then output T5;                                                        
      end;                                                                                           
    run;                                                                                             
    %macro each(range, template);                                                                    
                                                                                                     
    %mend;                                                                                           
    data freqs;                                                                                      
      merge t1(in=f5) t2(in=f4) t3(in=f3) t4(in=f2) t5(in=f1) end=end;                               
      by I;                                                                                          
      bin = input(cats(of f1-f5), binary5.);                                                         
      array freqs (0:31) _temporary_;                                                                
      array masks (0:31) _temporary_ (0:31);                                                         
      do _n_ = 0 to 31;                                                                              
        freqs(_n_) + band(masks(_n_), bin) = masks(_n_);                                             
      end;                                                                                           
      if end then do index=0 to 31;                                                                  
        cnt=freqs(index);                                                                            
        output;                                                                                      
      end;                                                                                           
      keep index cnt;                                                                                
    run;                                                                                             
    .                                                                                                
    Enjoy,                                                                                           
                                                                                                     
    Richard DeVenezia                                                                                
                                                                                                     
    *____             _                                                                              
    | __ )  __ _ _ __| |_                                                                            
    |  _ \ / _` | '__| __|                                                                           
    | |_) | (_| | |  | |_                                                                            
    |____/ \__,_|_|   \__|                                                                           
                                                                                                     
    ;                                                                                                
    Bart Jablonski <yabwon@GMAIL.COM>                                                                
                                                                                                     
    Hi Roger,                                                                                        
                                                                                                     
    I think that merge+array and binary5. format will work, below is the code for                    
    T1-T5, and some testing + macro (tested also on T1-T7, T7 is disjoint from T1-T6)                
    [everything is outputed into the log]                                                            
                                                                                                     
    All the best                                                                                     
    Bart                                                                                             
                                                                                                     
                                                                                                     
    data T1 T2 T3 T4 T5 ;                                                                            
      call streaminit(4321);                                                                         
      do I=1 to 30;                                                                                  
        if rand("uniform")>.6 then output T1;                                                        
        if rand("uniform")>.6 then output T2;                                                        
        if rand("uniform")>.6 then output T3;                                                        
        if rand("uniform")>.6 then output T4;                                                        
        if rand("uniform")>.6 then output T5;                                                        
      end;                                                                                           
    run;                                                                                             
                                                                                                     
    options ls=max ps=max;                                                                           
    data _null_;                                                                                     
      merge                                                                                          
        T1(in=T1)                                                                                    
        T2(in=T2)                                                                                    
        T3(in=T3)                                                                                    
        T4(in=T4)                                                                                    
        T5(in=T5)                                                                                    
      end = EOF;                                                                                     
      ;                                                                                              
      by I;                                                                                          
                                                                                                     
      array JJ[1:31] _temporary_;                                                                    
                                                                                                     
      length Jbin $ 5;                                                                               
      do J=1 to 31;                                                                                  
        Jbin = reverse(put(J, binary5.));                                                            
        /* if this I in given group */                                                               
        x = (                                                                                        
          char(Jbin,1) * T1 +                                                                        
          char(Jbin,2) * T2 +                                                                        
          char(Jbin,3) * T3 +                                                                        
          char(Jbin,4) * T4 +                                                                        
          char(Jbin,5) * T5                                                                          
          );                                                                                         
        /* how many group we are expecting */                                                        
        y = (                                                                                        
          char(Jbin,1) +                                                                             
          char(Jbin,2) +                                                                             
          char(Jbin,3) +                                                                             
          char(Jbin,4) +                                                                             
          char(Jbin,5)                                                                               
          );                                                                                         
                                                                                                     
        JJ[J] + (x=y);                                                                               
        /*put _all_;*/                                                                               
      end;                                                                                           
                                                                                                     
      if EOF then                                                                                    
      do J2 = 1 to 31;                                                                               
        J2bin = reverse(put(J2, binary5.));                                                          
        put J2=z2. J2bin= JJ[J2];                                                                    
      end;                                                                                           
    run;                                                                                             
                                                                                                     
                                                                                                     
                                                                                                     
                                                                                                     
    data T6;                                                                                         
      i = 30; output;                                                                                
      i = 31; output;                                                                                
    run;                                                                                             
                                                                                                     
    data T7;                                                                                         
      i = 32; output;                                                                                
      i = 33; output;                                                                                
    run;                                                                                             
                                                                                                     
                                                                                                     
    %macro test(                                                                                     
      t=5 /* number of sets to compare */                                                            
    , sort=0 /* if sorting is required ste to 1 */                                                   
    );                                                                                               
                                                                                                     
    %let N = %sysevalf(2**&t. - 1);                                                                  
    %let L = %sysfunc(ceil(%sysfunc(log10(&N.))));                                                   
                                                                                                     
    %if &sort. %then                                                                                 
    %do;                                                                                             
      %do I = 1 %to &t.;                                                                             
        proc sort data = T&I.;                                                                       
          by I;                                                                                      
        run;                                                                                         
      %end;                                                                                          
    %end;                                                                                            
                                                                                                     
    data _null_;                                                                                     
      merge                                                                                          
        %do I = 1 %to &t.;                                                                           
          T&I.(in=T&I.)                                                                              
        %end;                                                                                        
      end = EOF;                                                                                     
      ;                                                                                              
      by I;                                                                                          
                                                                                                     
      array JJ[1:&N.] _temporary_;                                                                   
                                                                                                     
      length Jbin $ &t.;                                                                             
      do J=1 to &N.;                                                                                 
        Jbin = reverse(put(J, binary&t..));                                                          
        /* if this I in given group */                                                               
        x = (                                                                                        
          %do I = 1 %to &t.;                                                                         
            char(Jbin,&I.) * T&I. +                                                                  
          %end;                                                                                      
          0                                                                                          
          );                                                                                         
        /* how many group we are expecting */                                                        
        y = (                                                                                        
          %do I = 1 %to &t.;                                                                         
            char(Jbin,&I.) +                                                                         
          %end;                                                                                      
          0                                                                                          
          );                                                                                         
                                                                                                     
        JJ[J] + (x=y);                                                                               
        /*put _all_;*/                                                                               
      end;                                                                                           
                                                                                                     
      if EOF then                                                                                    
      do J2 = 1 to &N.;                                                                              
        J2bin = reverse(put(J2, binary&t..));                                                        
        put J2=z&L.. J2bin= JJ[J2];                                                                  
      end;                                                                                           
    run;                                                                                             
                                                                                                     
    %mend test;                                                                                      
                                                                                                     
    %test(t=5)                                                                                       
                                                                                                     
    %test(t=7)                                                                                       


