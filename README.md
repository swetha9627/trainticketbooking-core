
# Project Features


# Feature 1- viewing train details:-

create table train_details (tr_id number NOT NULL,train_number int NOT NULL,train_name varchar2(50) NOT NULL,
				                     train_source varchar2(40) NOT NULL,train_destination varchar2(40) NOT NULL,
			                        source_time timestamp NOT NULL,destination_time timestamp NOT NULL,
			                         class_type varchar2(40) NOT NULL,fare int NOT NULL,
                                 unique(train_name),check(fare>0),
                                    constraint tr_id_pk primary key(tr_id)
			                      );
select * from train_details;

# ADD Query:-

insert into train_details values (1,21653,'TEJASEXPRESS','TRICHY','MADUARAI',to_timestamp('10-jun-2021 07:10:34','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('11-jun-2021 23:13:24','DD-Mon-YYYY HH24:MI:SS'),'FirstClass',1240);

insert into train_details values (2,21654,'VAIGAIEXPRESS','MADURAI','CHENNAI',to_timestamp('11-jun-2021 08:10:00','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('12-jun-2021 13:10:00','DD-Mon-YYYY HH24:MI:SS'),'SecondClass',1000);

insert into train_details values (3,21655,'TRICHYEXPRESS','CHENNAI','TRICHY',to_timestamp('12-jun-2021 10:10:00','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('13-jun-2021 16:10:00','DD-Mon-YYYY HH24:MI:SS'),'THRIDClass',750);

insert into train_details values (4,21656,'PACHAIYAPPAEXPRESS','COMIBATORE','TRIRUNELVELI',to_timestamp('14-jun-2021 23:10:00','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('15-jun-2021 05:10:00','DD-Mon-YYYY HH24:MI:SS'),'FirstClass',1150);

insert into train_details values (5,24567,'RAYAPURAMEXPRESS','TRICHY','CHENNAI',to_timestamp('15-Jul-2021 23:00:00','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('16-jul-2021 07:10:00','DD-Mon-YYYY HH24:MI:SS'),'FirstClass',1050);

insert into train_details values (6,24586,'VELANEXPRESS','MADUARI','SELAM',to_timestamp('23-Jul-2021 07:00:00','DD-Mon-YYYY HH24:MI:SS'),
to_timestamp('23-jul-2021 12:10:00','DD-Mon-YYYY HH24:MI:SS'),'SecondClass',830);

select * from train_details;

drop table train_details;

DELETE FROM train_details WHERE train_name='RAYAPURAMEXPRESS';

# Feature 2-passenger details:-

create table passenger_details
(
 passanger_id number ,
 tr_id number not null,
 passanger_name varchar2(100) not null,
 gender varchar2(10) not null,
 contact_number number not null,
 aadhar_number number not null unique,
constraint  gender_ch check(gender in('male','female','others')),
constraint passanger_id_pk primary key(passanger_id),
constraint train_id_fk foreign key(tr_id) references train_details(tr_id));

select * from  passenger_details;

# ADD Query:-

insert into passenger_details
values(1111,1,'swetha','female',9937808765,1000549873);

insert into passenger_details
values(2222,2,'mohamad','male',9876543210,8100054973);

insert into passenger_details
values(3333,3,'sakil','male',8765432109,6553476298);

insert into passenger_details
values(4444,4,'nivetha','female',8765437809,9876234768);

drop table passenger_details;

# Feature- 3 booking details

create table booking_details
(
tr_id number,
passanger_id number,
pnr_no number primary key,
booking_date date,
compartment_no varchar2(10)not null,
coach_type varchar2(20) not null,
birth_type varchar2(20) not null,
constraint passanger_id_fk foreign key(passanger_id) references passenger_details(passanger_id), 
constraint tr_id_fk foreign key(tr_id) references train_details(tr_id));

select * from booking_details;

# ADD Query:-

insert into booking_details values(1,1111,100001,'30/july/2021','S-3','NON_AC','sitting');

insert into booking_details values(2,2222,100002,'22/july/2021','S-5','NON_AC','sleeper');

insert into booking_details values(3,3333,100003,'26/july/2021','S-1','AC','ac_sleeper');

drop table booking_details;

# Feature -4 viewing all train names:-

select train_name from train_details;

# Feature -5 displaying the female passengers with their train name:-

select p.passanger_name,t.train_name from passenger_details p ,train_details t where p.tr_id=t.tr_id and p.gender='female';

# Feature -6 giving name of the train we can able to see where the train starts and ends and finally displaying it in ascending order

select train_name,train_source,train_destination from train_details order by train_name;

# Feature -7 count of trains moving from chennai

select count(train_source) from train_details where train_source='CHENNAI';

# Feature -8 altering the booking_table column and adding the column to know the status of payment_status

alter table booking_details add payment_status varchar2(100);

update  booking_details set payment_status='paid' where tr_id=1;

update  booking_details set payment_status='not_paid' where tr_id=2;

update booking_details set payment_status='paid' where tr_id=3;

select * from booking_details;
