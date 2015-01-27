# SQL_CPP_Example

This program does four different queries and outputs the results. This program connects to an sqlplus database and extracts the necessary data. Here is the relevant SQL for the different querries

    Find the names of aircraft such that all pilots certified to operate them have salaries over $80,000. 
          void buildOption1() { 
            strcpy((char *)sql_statement.arr,"select aname from aircraft where aid in ");
            strcat((char *)sql_statement.arr,"(select certified.aid from employees, certified ");
            strcat((char *)sql_statement.arr,"where certified.eid = employees.eid and employees.salary > 80000)");
            }
           
    For each pilot who is certified for more than X1 aircrafts, find the eid and the maximum cruisingrange of the aircraft
    for which she or he is certified.
          void buildOption2(string x1) {
            strcpy((char *)sql_statement.arr,"select eid, cruisingrange from aircraft, certified where ");
      	    strcat((char *)sql_statement.arr,"aircraft.aid = certified.aid and certified.eid in ");
      	    strcat((char *)sql_statement.arr,"(select eid from certified group by eid ");
      	    strcat((char *)sql_statement.arr,"having(count(eid) > ");
      	    strcat((char *)sql_statement.arr,x1.c_str());
      	    strcat((char *)sql_statement.arr,")) order by eid;");
            cout << "Query is  " << (char *) sql_statement.arr << endl;
            }
            
    For all aircraft with cruising range over X2 miles, find the name of the aircraft and the average salary of all pilots       certified for this aircraft.
          void buildOption3(string x2) {
            strcpy((char *)sql_statement.arr,"select aname,(select cast(avg(employees.salary) ");
            strcat((char *)sql_statement.arr,"as decimal (25,2)) from employees,certified ");
            strcat((char *)sql_statement.arr,"where certified.eid=employees.eid and certified.aid=aircraft.aid) as average");
            strcat((char *)sql_statement.arr,"from aircraft where cruisingrange > ");
            strcat((char *)sql_statement.arr,x2.c_str());
            }
            
    Find the aids of all aircraft that can be used on routes from X3 to X4
          void buildOption4(string x3, string x4) {
            strcpy((char *)sql_statement.arr,"select aid ");
            strcat((char *)sql_statement.arr,"from aircraft, flights ");
            strcat((char *)sql_statement.arr,"where aircraft.cruisingrange > flights.distance ");
            strcat((char *)sql_statement.arr,"and f_from = '");
            strcat((char *)sql_statement.arr,x3.c_str());
            strcat((char *)sql_statement.arr,"' and f_to = '");
            strcat((char *)sql_statement.arr,x4.c_str());
            strcat((char *)sql_statement.arr,"'");
           }


Unfortunately option #2 does not work and returns an SQL error -918
