Practical 1A: - Temporal Database
CREATE TABLE SHARES(COMPANY_NAME VARCHAR(10),NO_SHARES NUMBER(5),PRICE NUMBER(5),TRANSACTION_TIME TIMESTAMP);

insert into shares values('&company_name',&no_shares,&price,'&transaction_time');

–example values
Company: Infosys, No_Shares: 300, Price: 10, Transaction_Time: '10-FEB-10 09.20.45.000000 AM'
Company: Wipro, No_Shares: 200, Price: 20, Transaction_Time: '08-JUL-11 02.30.12.000000 PM'
Company: Himalaya, No_Shares: 100, Price: 15, Transaction_Time: '18-NOV-09 12.40.42.000000 AM'
Company: MBT, No_Shares: 100, Price: 20, Transaction_Time: '28-APR-11 01.00.42.000000 PM'
Company: Patni, No_Shares: 500, Price: 10, Transaction_Time: '28-APR-11 10.07.24.000000 PM'
Company: IGate, No_Shares: 40, Price: 5, Transaction_Time: '10-JUN-10 04.25.48.000000 AM'
Company: Capgemini, No_Shares: 250, Price: 5, Transaction_Time: '23-FEB-13 10.00.55.000000 PM'
Company: Infosys, No_Shares: 300, Price: 50, Transaction_Time: '22-JUN-11 10.07.50.000000 AM'
Company: Patni, No_Shares: 100, Price: 20, Transaction_Time: '21-JUN-14 10.07.45.000000 PM'
Company: Infosys, No_Shares: 150, Price: 40, Transaction_Time: '14-DEC-15 09.15.45.000000 AM'

select company_name from shares where price >=10 and to_char(transaction_time,'hh12:mi') = '10:07';



select company_name,no_shares from shares where to_char(transaction_time,'dd-mm-yyyyHh12:mi:am')>= '22-sept-2011 10:07:am';





Practical 1B: - Temporal Database


create table time_emp(accno varchar(5),ename varchar(10),jdate date,rdate date);

INSERT INTO time_emp VALUES('&accno', '&ename', TO_DATE('&jdate', 'YYYY-MM-DD'), TO_DATE('&rdate', 'YYYY-MM-DD'));

select * from time_emp where jdate='22-OCT-2015';


select * from time_emp where rdate='22-SEP-2021';


select * from time_emp where rdate=to_date('22-SEP-2021','dd-mm-yyyy');


select accno,ename,jdate from time_emp where jdate between '1-jan-2016' and '31-dec-2024';


select * from time_emp where rdate between '1-oct-2021' and '31-dec-2025';


select * from time_emp where rdate between sysdate and sysdate+365;


 select * from time_emp where ename in('Jacob','Bob') and jdate between '1-jun-2009' and '8-dec-2025';





Practical 2 - Activate Database
create table employee_01(Eno number primary key,Ename varchar2(10),hours number,p_no number,super_no number);

Insert into employee_01 values(&Eno, ‘&Ename’, &hours, &p_no, &super_no);

Example values:
(1, 'seema', 12, 11, 2),
(2, 'smita', 15, 11, 10),
(3, 'sarika', 14, 11, 2),
(4, 'sujata', 12, 12, 34),
(5, 'raji', 10, 12, 34),
(6, 'rekha', 15, 12, 34),
(7, 'minal', 10, 13, 67),
(8, 'sheetal', 5, 13, 67),
(9, 'hetal', 15, 13, 67),
(10, 'neha', 14, 14, 78);

create table project_01(p_no number primary key,pname varchar2(10),tothrs number,super_no number);

Insert INTO project_01 values(&P_NO, ‘&PNAME’, &TOTHRS, &SUPER_NO)

Example values:
(11, 'ABC', 0, 2),
(12, 'PQR', 0, 34),
(13, 'XYZ', 0, 67),
(14, 'LMN', 0, 78);

UPDATE TOTHRS IN TABLE PROJECT (perform same for p_no=12 and p_no=13)
update project_01 set tothrs=tothrs+(select sum(hours) from employee_01 where p_no=11) where p_no=11;

– Creating Trigger to insert new employees tuple and display the new total hours from project table.

create or replace trigger emp_pr_01
after insert on employee_01
for each row
when(new.p_no IS NOT NULL)
begin
update project_01
set tothrs=tothrs+:new.hours
where p_no=:new.p_no;
end;
/

insert into employee_01 values(11,'snehal',20,14,78);
select * from employee_01;
select * from project_01;


– Creating a trigger to change the hours of existing employee and display the new total hours from project table.

create or replace trigger emp_pr_02
after update of hours on employee_01
for each row
when(old.p_no IS NOT NULL)
begin
update Project_01
set tothrs=tothrs-:old.hours+:new.hours
where p_no=:old.p_no;
end;
/

update employee_01 set hours=50 where ename ='snehal';
select * from employee_01;
select * from project_01;


update employee_01 set p_no=14, super_no=78 where ename='smita';
select * from employee_01;
select * from project_01;


– Create a trigger to delete the project of an employee.
create or replace trigger emp_pr_04
after delete on employee_01
for each row
when(old.p_no IS NOT NULL)
begin
update project_01
set tothrs=tothrs-:old.hours
where p_no=:old.p_no;
end;
/

update employee_01 set p_no=null where ename='sheetal';
select * from employee_01;
select * from project_01;



























Practical 3:- Deduction Database
A) factorial 
factorial (0,1).
factorial(N,F):- N>0,N1 is N-1,factorial(N1,F1),F is N*F1

B) Tower of Hanoi
move(1,X,Y,_) -
write('Move top disk from '),
write(X),
write(' to '),
write(Y),
nl.
move(N,X,Y,Z) -
N1,
M is N-1,
move(M,X,Z,Y),
move(1,X,Y,_),
move(M,Z,Y,X).




Practical 4:- ORDBMS Application
create or replace type AddrType as object(Pincode number(5),Street char(20),City varchar2(50),state varchar2(40),no number(4));/

create or replace type BranchType as object(address AddrType,phone1 integer,phone2 integer);/

create or replace type BranchTableType as table of BranchType;/

create or replace type AuthorType as object( name varchar2(50), addr AddrType);/

create table Authors of AuthorType;

create or replace type AuthorListType as varray(10) of ref AuthorType;/

create or replace type PublisherType as object(name varchar2(50), addr AddrType, branches BranchTableType);/

create table Publishers of PublisherType NESTED TABLE branches STORE as BranchTable;

create table books( title varchar2(50), year date, published_by ref PublisherType, Authors AuthorListType);

insert into Authors values ('Rabiner', AddrType(5002,'sstreet','pune','mha',04));

insert into authors values ('Jerry', AddrType(7003, 'dstreet','mumbai','mha',1003));

insert into authors values ('Paulraj', AddrType(7008, 'sstreet','mumbai','mha',1007));

insert into authors values ('Elmasri', AddrType(7006, 'nstreet','mumbai','mha',1006));

insert into authors values ('Ramakrishnan', AddrType(8002, 'dstreet','pune','mha',1003));

insert into Authors values ('Schiller', AddrType(7002,'lbsmarg','mumbai','mha',01));

insert into Publishers values('Pearson', AddrType(4002,'rstreet','mumbai','mha',03), BranchTableType (BranchType (AddrType(5002, 'fstreet','mumbai','mha', 03),23406,69896)));

insert into Publishers values( 'ekta', AddrType(7007, 'sstreet','mumbai','mha',1007), BranchTableType (BranchType( AddrType(7007, 'sstreet','mumbai','mha',1007),4543545,8676775)));

insert into Publishers values( 'joshi', AddrType(7008, 'sstreet','mumbai','mha',1007), BranchTableType (BranchType( AddrType(1002, 'sstreet','nasik','mha',1007), 456767,7675757)));

insert into Publishers values( 'wiley', AddrType(6002, 'sstreet','nasik','mha',1007), BranchTableType(BranchType( AddrType(6002, 'sstreet','nasik','mha',1007),4543545,8676775)));

insert into books select 'DSP','28-may-1983',ref(pub), AuthorListType(ref(aut)) from Publishers pub,Authors aut where pub.name='joshi' and aut.name='Elmasri';

insert into books select 'compiler','09-jan-1890',ref(pub), AuthorListType(ref(aut)) from Publishers pub,Authors aut where pub.name='wiley'and aut.name='Jerry';

insert into books select 'Speech Recognition','25-may-1983',ref(pub), AuthorListType(ref(aut)) from Publishers pub,Authors aut where pub.name='Pearson'and aut.name='Rabiner';

insert into books select 'DBMS','28-may-1983',ref(pub), AuthorListType(ref(aut)) from Publishers pub, Authors aut where pub.name='joshi' and aut.name='Elmasri';

insert into books select 'DBMS','28-may-1983',ref(pub), AuthorListType(ref(aut)) from Publishers pub, Authors aut where pub.name='Pearson' and aut.name='Elmasri';

insert into books select 'DSP', '28-may-1983',ref(pub), AuthorListType(ref(aut)) from publishers pub, Authors aut where pub.name='joshi' and aut.name='Jerry';

– List all of the authors that have the same address as their publisher:

select aut.name from Authors aut ,Publishers pub where aut.addr=pub.addr;


– List all of the authors that have the same pin code as their publisher:

select aut.name from Authors aut ,Publishers pub where aut.addr.pincode=pub.addr.pincode;

– List all books that have 2 or more authors:

select * from books b where 1 = ( select count(*)from table (b.authors));


– List the title of the book that has the most authors:

select title from books b, table(b.authors) group by title having count(*) = (select max(count(*)) from books b, table(b.authors) group by title);

– List the name of the publisher that has the most branches:

Select p.name from publishers p, table(p.branches) group by p.name having count(*)> = all (select count(*) from publishers p, table(p.branches) group by name);


– Name of authors who have not published a book:

select a.name from authors a where not exists( select b.title from books b, table(b.authors) where a.name = name);


– Move all the branches that belong to the publisher 'wiley' to the publisher ‘ekta'.

insert into table( select branches from publishers where name = 'wiley') select b.* from publishers p, table(p.branches) b where name = 'ekta';


– List all authors who have published more than one book:

select a.name from authors a, books b,table(b.authors) v where v.column_value=ref(a) group by a.name;


– List all books (title) where the same author appears more than once on the list of authors (assuming that an integrity constraint requiring that the name of an author is unique in a list of authors has not been specified).

select title from authors a, books b, table(b.authors) v where v.column_value = ref(a) group by title having count(*) > 1;

Practical 5 - XML Database
Create table emp1(dept_id number(4), emp_spec XMLType);

insert into emp1 values(1001,XMLType('<employees><emp id="001">
<name>Alpha</name>
<email>alpha@newton.com</email>
<acc_no>101</acc_no>
<dateofjoining>1995-11-23</dateofjoining>
</emp></employees>'));

insert into emp1 values(1001,XMLType('<employees><emp id="002">
<name>Beta</name>
<email>Beta@newton.com</email>
<acc_no>102</acc_no>
<dateofjoining>1994-09-15</dateofjoining>
</emp></employees>'));

insert into emp1 values(1001,XMLType('<employees><emp id="003">
<name>Gamma</name>
<email>Gamma@newton.com</email>
<acc_no>103</acc_no>
<dateofjoining>1994-08-30</dateofjoining>
</emp></employees>'));

– Retrieve the names of employee from employee table 

select e.emp_spec.extract('/employees/emp/name/text()').getStringVal() "names" from emp1 e;


– Retrieve the acc_no  of employee from employee table

select e.emp_spec.extract('/employees/emp/acc_no/text()').getStringVal() "acc_no" from emp1 e;


– Retrieve the names, acc_no, email of employees from employee table

select e.emp_spec.extract('/employees/emp/name/text()').getStringVal()
"names",e.emp_spec.extract('/employees/emp/acc_no/text()').getStringVal()
"acc_no",e.emp_spec.extract('/employees/emp/email/text()').getStringVal()
"email" from emp1 e;


– How to update XML

update emp1 e set emp_spec=XMLType('<employees>
<emp id="002">
<name>Omega</name>
</emp>
</employees>')
where e.emp_spec.extract('/employees/emp/@id').getStringVal()='002';

– Delete an XML Type Column Row
delete from emp1 e where e.emp_spec.extract('/employees/emp/@id').getStringVal()='003';

select * from emp1;




Practical 6 - Create XML Parser
– Create .html file and enter following code:

<!DOCTYPE html>
<html>
<body>
<p id="demo"></p>
<script>
var x, i ,xmlDoc;
var txt = "";
var text = "<book>" + "<title>Everyday Italian</title>" +
"<author>Giada De Laurentiis</author>" + "<year>2005</year>" + "</book>";
parser = new DOMParser();
xmlDoc = parser.parseFromString(text,"text/xml");
x = xmlDoc.documentElement.childNodes;
for (i = 0; i < x.length ;i++) {
txt += x[i].nodeName + ": " + x[i].childNodes[0].nodeValue + "<br>";
}
document.getElementById("demo").innerHTML = txt;
</script>
</body>
</html>

– Open html file in browser for output:

