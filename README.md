# DBMS
mysql> CREATE DATABASE iet2;

mysql> USE iet2;

mysql> CREATE TABLE Routes (
    RouteID INT PRIMARY KEY AUTO_INCREMENT,
    RouteName VARCHAR(100),
    Distance FLOAT,
    TimeTaken VARCHAR(20),
    OperatingDays VARCHAR(50)
);

mysql> CREATE TABLE TrainSchedules (
    ScheduleID INT PRIMARY KEY AUTO_INCREMENT,
    RouteID INT,
    DepartureStation VARCHAR(100),
    ArrivalStation VARCHAR(100),
    DepartureTime TIME,
    ArrivalTime TIME,
    DaysOfOperation VARCHAR(50),
    FOREIGN KEY (RouteID) REFERENCES Routes(RouteID)
);

mysql> CREATE TABLE Trains (
    TrainID INT PRIMARY KEY AUTO_INCREMENT,
    TrainName VARCHAR(100),
    SeatingCapacity INT,
    Mileage FLOAT,
    LastMaintenanceDate DATE
);

mysql> CREATE TABLE Drivers (
    DriverID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100),
    ContactInformation VARCHAR(100),
    RestDay VARCHAR(20),
    HomeCity VARCHAR(100)
);

mysql> CREATE TABLE Coaches (
    CoachID INT PRIMARY KEY AUTO_INCREMENT,
    TrainID INT,
    DriverID INT,
    CoDriverID INT,
    DepartureTime TIME,
    ArrivalTime TIME,
    StandbyActivated BOOLEAN,
    FOREIGN KEY (TrainID) REFERENCES Trains(TrainID),
    FOREIGN KEY (DriverID) REFERENCES Drivers(DriverID),
    FOREIGN KEY (CoDriverID) REFERENCES Drivers(DriverID)
);

mysql> CREATE TABLE TravelAgents (
    TravelAgentID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100),
    CommissionRate DECIMAL(5, 2)
);

mysql> CREATE TABLE Passengers (
    PassengerID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100),
    Age INT,
    RouteID INT,
    BookingDate DATE,
    TravelAgentID INT,
    FOREIGN KEY (RouteID) REFERENCES Routes(RouteID),
    FOREIGN KEY (TravelAgentID) REFERENCES TravelAgents(TravelAgentID)
);

mysql> show tables;
+----------------+
| Tables_in_iet2 |
+----------------+
| coaches        |
| drivers        |
| passengers     |
| routes         |
| trains         |
| trainschedules |
| travelagents   |
+----------------+

mysql> INSERT INTO Routes (RouteID, RouteName, Distance, TimeTaken, OperatingDays)
VALUES
(1, 'Mumbai Central - Gandhinagar', 548, '5 hours 40 minutes', 'Mon, Tue, Wed, Thu, Fri, Sat'),
(2, 'New Delhi - Himachal', 412, '6 hours 10 minutes', 'Mon, Tue, Wed, Fri, Sat'),
(3, 'Secunderabad - Visakhapatnam', 500, '8 hours 30 minutes', 'Sun'),
(4, 'Mumbai - Sainagar Shirdi', 248, '5 hours 20 minutes', 'Mon, Wed, Thu, Fri, Sat'),
(5, 'Mumbai - Solapur', 400, '6 hours 35 minutes', 'Mon, Tue, Thu, Fri, Sat'),
(6, 'Bhopal - Delhi', 700, '7 hours 45 minutes', 'Mon, Tue, Wed, Thu, Fri'),
(7, 'Lonavala - Ajmer', 1062, '10 hours 45 minutes', 'Sat'),
(8, 'Dharwad - Bengaluru', 432, '5 hours', 'Mon, Wed, Sat'),
(9, 'Bhopal - Indore', 246, '6 hours', 'Mon, Tue, Wed, Thu, Fri, Sat, Sun'),
(10, 'Mumbai - Goa', 588, '3 hours', 'Mon, Tue, Wed, Thu, Fri, Sat, Sun');

mysql> desc Routes;
+---------------+--------------+------+-----+---------+----------------+
| Field         | Type         | Null | Key | Default | Extra          |
+---------------+--------------+------+-----+---------+----------------+
| RouteID       | int          | NO   | PRI | NULL    | auto_increment |
| RouteName     | varchar(100) | YES  |     | NULL    |                |
| Distance      | float        | YES  |     | NULL    |                |
| TimeTaken     | varchar(20)  | YES  |     | NULL    |                |
| OperatingDays | varchar(50)  | YES  |     | NULL    |                |
+---------------+--------------+------+-----+---------+----------------+

mysql> select* from Routes;
+---------+------------------------------+----------+---------------------+-----------------------------------+
| RouteID | RouteName                    | Distance | TimeTaken           | OperatingDays                     |
+---------+------------------------------+----------+---------------------+-----------------------------------+
|       1 | Mumbai Central - Gandhinagar |      548 | 5 hours 40 minutes  | Mon, Tue, Wed, Thu, Fri, Sat      |
|       2 | New Delhi - Himachal         |      412 | 6 hours 10 minutes  | Mon, Tue, Wed, Fri, Sat           |
|       3 | Secunderabad - Visakhapatnam |      500 | 8 hours 30 minutes  | Sun                               |
|       4 | Mumbai - Sainagar Shirdi     |      248 | 5 hours 20 minutes  | Mon, Wed, Thu, Fri, Sat           |
|       5 | Mumbai - Solapur             |      400 | 6 hours 35 minutes  | Mon, Tue, Thu, Fri, Sat           |
|       6 | Bhopal - Delhi               |      700 | 7 hours 45 minutes  | Mon, Tue, Wed, Thu, Fri           |
|       7 | Lonavala - Ajmer             |     1062 | 10 hours 45 minutes | Sat                               |
|       8 | Dharwad - Bengaluru          |      432 | 5 hours             | Mon, Wed, Sat                     |
|       9 | Bhopal - Indore              |      246 | 6 hours             | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
|      10 | Mumbai - Goa                 |      588 | 3 hours             | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
+---------+------------------------------+----------+---------------------+-----------------------------------+

mysql> INSERT INTO TrainSchedules (ScheduleID, RouteID, DepartureStation, ArrivalStation, DepartureTime, ArrivalTime, DaysOfOperation)
VALUES
(1, 1, 'Mumbai Central', 'Gandhinagar', '05:00:00', '10:40:00', 'Mon, Tue, Wed, Thu, Fri, Sat'),
(2, 2, 'New Delhi', 'Himachal', '06:00:00', '12:10:00', 'Mon, Tue, Wed, Fri, Sat'),
(3, 3, 'Secunderabad', 'Visakhapatnam', '08:00:00', '16:30:00', 'Sun'),
(4, 4, 'Mumbai', 'Sainagar Shirdi', '07:00:00', '12:20:00', 'Mon, Wed, Thu, Fri, Sat'),
(5, 5, 'Mumbai', 'Solapur', '09:00:00', '15:35:00', 'Mon, Tue, Thu, Fri, Sat'),
(6, 6, 'Bhopal', 'Delhi', '06:30:00', '14:15:00', 'Mon, Tue, Wed, Thu, Fri'),
(7, 7, 'Lonavala', 'Ajmer', '08:00:00', '18:45:00', 'Sat'),
(8, 8, 'Dharwad', 'Bengaluru', '09:00:00', '14:00:00', 'Mon, Wed, Sat'),
(9, 9, 'Bhopal', 'Indore', '07:30:00', '13:30:00', 'Mon, Tue, Wed, Thu, Fri, Sat, Sun'),
(10, 10, 'Mumbai', 'Goa', '08:00:00', '11:00:00', 'Mon, Tue, Wed, Thu, Fri, Sat, Sun');

mysql> desc TrainSchedules;
+------------------+--------------+------+-----+---------+----------------+
| Field            | Type         | Null | Key | Default | Extra          |
+------------------+--------------+------+-----+---------+----------------+
| ScheduleID       | int          | NO   | PRI | NULL    | auto_increment |
| RouteID          | int          | YES  | MUL | NULL    |                |
| DepartureStation | varchar(100) | YES  |     | NULL    |                |
| ArrivalStation   | varchar(100) | YES  |     | NULL    |                |
| DepartureTime    | time         | YES  |     | NULL    |                |
| ArrivalTime      | time         | YES  |     | NULL    |                |
| DaysOfOperation  | varchar(50)  | YES  |     | NULL    |                |
+------------------+--------------+------+-----+---------+----------------+

mysql> select* from TrainSchedules;
+------------+---------+------------------+-----------------+---------------+-------------+-----------------------------------+
| ScheduleID | RouteID | DepartureStation | ArrivalStation  | DepartureTime | ArrivalTime | DaysOfOperation                   |
+------------+---------+------------------+-----------------+---------------+-------------+-----------------------------------+
|          1 |       1 | Mumbai Central   | Gandhinagar     | 05:00:00      | 10:40:00    | Mon, Tue, Wed, Thu, Fri, Sat      |
|          2 |       2 | New Delhi        | Himachal        | 06:00:00      | 12:10:00    | Mon, Tue, Wed, Fri, Sat           |
|          3 |       3 | Secunderabad     | Visakhapatnam   | 08:00:00      | 16:30:00    | Sun                               |
|          4 |       4 | Mumbai           | Sainagar Shirdi | 07:00:00      | 12:20:00    | Mon, Wed, Thu, Fri, Sat           |
|          5 |       5 | Mumbai           | Solapur         | 09:00:00      | 15:35:00    | Mon, Tue, Thu, Fri, Sat           |
|          6 |       6 | Bhopal           | Delhi           | 06:30:00      | 14:15:00    | Mon, Tue, Wed, Thu, Fri           |
|          7 |       7 | Lonavala         | Ajmer           | 08:00:00      | 18:45:00    | Sat                               |
|          8 |       8 | Dharwad          | Bengaluru       | 09:00:00      | 14:00:00    | Mon, Wed, Sat                     |
|          9 |       9 | Bhopal           | Indore          | 07:30:00      | 13:30:00    | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
|         10 |      10 | Mumbai           | Goa             | 08:00:00      | 11:00:00    | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
+------------+---------+------------------+-----------------+---------------+-------------+-----------------------------------+

mysql> INSERT INTO Trains (TrainID, TrainName, SeatingCapacity, Mileage, LastMaintenanceDate)
VALUES
(1, 'Express 1', 30, 10000, '2023-08-15'),
(2, 'Superfast 2', 30, 8000, '2023-09-20'),
(3, 'Speedster 3', 30, 12000, '2023-07-10'),
(4, 'RapidTransit 4', 30, 6000, '2023-11-05'),
(5, 'SwiftExpress 5', 30, 9000, '2023-10-30'),
(6, 'TurboCharge 6', 30, 11000, '2023-06-25'),
(7, 'MegaRider 7', 30, 7500, '2023-08-28'),
(8, 'UltraSwift 8', 30, 9500, '2023-10-12'),
(9, 'RocketSpeed 9', 30, 8500, '2023-09-05'),
(10, 'FastTrack 10', 30, 7000, '2023-11-20');

mysql> desc Trains;
+---------------------+--------------+------+-----+---------+----------------+
| Field               | Type         | Null | Key | Default | Extra          |
+---------------------+--------------+------+-----+---------+----------------+
| TrainID             | int          | NO   | PRI | NULL    | auto_increment |
| TrainName           | varchar(100) | YES  |     | NULL    |                |
| SeatingCapacity     | int          | YES  |     | NULL    |                |
| Mileage             | float        | YES  |     | NULL    |                |
| LastMaintenanceDate | date         | YES  |     | NULL    |                |
+---------------------+--------------+------+-----+---------+----------------+

mysql> select* from Trains;
+---------+----------------+-----------------+---------+---------------------+
| TrainID | TrainName      | SeatingCapacity | Mileage | LastMaintenanceDate |
+---------+----------------+-----------------+---------+---------------------+
|       1 | Express 1      |              30 |   10000 | 2023-08-15          |
|       2 | Superfast 2    |              30 |    8000 | 2023-09-20          |
|       3 | Speedster 3    |              30 |   12000 | 2023-07-10          |
|       4 | RapidTransit 4 |              30 |    6000 | 2023-11-05          |
|       5 | SwiftExpress 5 |              30 |    9000 | 2023-10-30          |
|       6 | TurboCharge 6  |              30 |   11000 | 2023-06-25          |
|       7 | MegaRider 7    |              30 |    7500 | 2023-08-28          |
|       8 | UltraSwift 8   |              30 |    9500 | 2023-10-12          |
|       9 | RocketSpeed 9  |              30 |    8500 | 2023-09-05          |
|      10 | FastTrack 10   |              30 |    7000 | 2023-11-20          |
+---------+----------------+-----------------+---------+---------------------+

mysql> INSERT INTO Drivers (DriverID, Name, ContactInformation, RestDay, HomeCity)
VALUES
(1, 'John Smith', '123-456-7890', 'Sunday', 'Mumbai'),
(2, 'Emily Johnson', '987-654-3210', 'Monday', 'Delhi'),
(3, 'Michael Davis', '555-123-4567', 'Tuesday', 'Chennai'),
(4, 'Sophia Brown', '777-888-9999', 'Wednesday', 'Kolkata'),
(5, 'William Wilson', '111-222-3333', 'Thursday', 'Bengaluru'),
(6, 'Olivia Miller', '444-555-6666', 'Friday', 'Hyderabad'),
(7, 'Daniel Martinez', '999-888-7777', 'Saturday', 'Ahmedabad'),
(8, 'Ava Garcia', '666-777-8888', 'Sunday', 'Pune'),
(9, 'James Moore', '222-333-4444', 'Monday', 'Jaipur'),
(10, 'Emma Taylor', '777-999-1111', 'Tuesday', 'Lucknow');

mysql> desc Drivers;
+--------------------+--------------+------+-----+---------+----------------+
| Field              | Type         | Null | Key | Default | Extra          |
+--------------------+--------------+------+-----+---------+----------------+
| DriverID           | int          | NO   | PRI | NULL    | auto_increment |
| Name               | varchar(100) | YES  |     | NULL    |                |
| ContactInformation | varchar(100) | YES  |     | NULL    |                |
| RestDay            | varchar(20)  | YES  |     | NULL    |                |
| HomeCity           | varchar(100) | YES  |     | NULL    |                |
+--------------------+--------------+------+-----+---------+----------------+

mysql> select* from Drivers;
+----------+-----------------+--------------------+-----------+-----------+
| DriverID | Name            | ContactInformation | RestDay   | HomeCity  |
+----------+-----------------+--------------------+-----------+-----------+
|        1 | John Smith      | 123-456-7890       | Sunday    | Mumbai    |
|        2 | Emily Johnson   | 987-654-3210       | Monday    | Delhi     |
|        3 | Michael Davis   | 555-123-4567       | Tuesday   | Chennai   |
|        4 | Sophia Brown    | 777-888-9999       | Wednesday | Kolkata   |
|        5 | William Wilson  | 111-222-3333       | Thursday  | Bengaluru |
|        6 | Olivia Miller   | 444-555-6666       | Friday    | Hyderabad |
|        7 | Daniel Martinez | 999-888-7777       | Saturday  | Ahmedabad |
|        8 | Ava Garcia      | 666-777-8888       | Sunday    | Pune      |
|        9 | James Moore     | 222-333-4444       | Monday    | Jaipur    |
|       10 | Emma Taylor     | 777-999-1111       | Tuesday   | Lucknow   |
+----------+-----------------+--------------------+-----------+-----------+

mysql> INSERT INTO Coaches (CoachID, TrainID, DriverID, CoDriverID, DepartureTime, ArrivalTime, StandbyActivated)
VALUES
(1, 1, 1, 2, '05:00:00', '10:40:00', 0),
(2, 2, 3, 4, '06:00:00', '12:10:00', 1),
(3, 3, 5, 6, '07:00:00', '14:30:00', 0),
(4, 4, 7, 8, '08:00:00', '13:20:00', 1),
(5, 5, 9, 10, '09:30:00', '16:05:00', 0),
(6, 6, 1, 3, '10:15:00', '17:00:00', 1),
(7, 7, 2, 4, '11:20:00', '18:30:00', 0),
(8, 8, 3, 5, '12:45:00', '19:15:00', 1),
(9, 9, 4, 6, '14:00:00', '21:20:00', 0),
(10, 10, 5, 7, '15:30:00', '22:45:00', 1);

mysql> desc Coaches;
+------------------+------------+------+-----+---------+----------------+
| Field            | Type       | Null | Key | Default | Extra          |
+------------------+------------+------+-----+---------+----------------+
| CoachID          | int        | NO   | PRI | NULL    | auto_increment |
| TrainID          | int        | YES  | MUL | NULL    |                |
| DriverID         | int        | YES  | MUL | NULL    |                |
| CoDriverID       | int        | YES  | MUL | NULL    |                |
| DepartureTime    | time       | YES  |     | NULL    |                |
| ArrivalTime      | time       | YES  |     | NULL    |                |
| StandbyActivated | tinyint(1) | YES  |     | NULL    |                |
+------------------+------------+------+-----+---------+----------------+

mysql>  select* from coaches;
+---------+---------+----------+------------+---------------+-------------+------------------+
| CoachID | TrainID | DriverID | CoDriverID | DepartureTime | ArrivalTime | StandbyActivated |
+---------+---------+----------+------------+---------------+-------------+------------------+
|       1 |       1 |        1 |          2 | 05:00:00      | 10:40:00    |                0 |
|       2 |       2 |        3 |          4 | 06:00:00      | 12:10:00    |                1 |
|       3 |       3 |        5 |          6 | 07:00:00      | 14:30:00    |                0 |
|       4 |       4 |        7 |          8 | 08:00:00      | 13:20:00    |                1 |
|       5 |       5 |        9 |         10 | 09:30:00      | 16:05:00    |                0 |
|       6 |       6 |        1 |          3 | 10:15:00      | 17:00:00    |                1 |
|       7 |       7 |        2 |          4 | 11:20:00      | 18:30:00    |                0 |
|       8 |       8 |        3 |          5 | 12:45:00      | 19:15:00    |                1 |
|       9 |       9 |        4 |          6 | 14:00:00      | 21:20:00    |                0 |
|      10 |      10 |        5 |          7 | 15:30:00      | 22:45:00    |                1 |
+---------+---------+----------+------------+---------------+-------------+------------------+

mysql> INSERT INTO TravelAgents (TravelAgentID, Name, CommissionRate)
VALUES
(1, 'ABC Travels', 10.0),
(2, 'XYZ Tours', 10.0),
(3, 'PQR Holidays', 10.0),
(4, 'LMN Travel Solutions', 10.0),
(5, 'JKL Voyages', 10.0),
(6, 'DEF Travel Agency', 10.0),
(7, 'GHI Trips', 10.0),
(8, 'NOP Excursions', 10.0),
(9, 'RST Tours and Travels', 10.0),
(10, 'UVW Vacation Planners', 10.0);

mysql> desc TravelAgents;
+----------------+--------------+------+-----+---------+----------------+
| Field          | Type         | Null | Key | Default | Extra          |
+----------------+--------------+------+-----+---------+----------------+
| TravelAgentID  | int          | NO   | PRI | NULL    | auto_increment |
| Name           | varchar(100) | YES  |     | NULL    |                |
| CommissionRate | decimal(5,2) | YES  |     | NULL    |                |
+----------------+--------------+------+-----+---------+----------------+

 mysql> select* from TravelAgents;
+---------------+-----------------------+----------------+
| TravelAgentID | Name                  | CommissionRate |
+---------------+-----------------------+----------------+
|             1 | ABC Travels           |          10.00 |
|             2 | XYZ Tours             |          10.00 |
|             3 | PQR Holidays          |          10.00 |
|             4 | LMN Travel Solutions  |          10.00 |
|             5 | JKL Voyages           |          10.00 |
|             6 | DEF Travel Agency     |          10.00 |
|             7 | GHI Trips             |          10.00 |
|             8 | NOP Excursions        |          10.00 |
|             9 | RST Tours and Travels |          10.00 |
|            10 | UVW Vacation Planners |          10.00 |
+---------------+-----------------------+----------------+

mysql> INSERT INTO Passengers (PassengerID, Name, Age, RouteID, BookingDate, TravelAgentID)
VALUES
(1, 'Alice Johnson', 25, 1, '2023-11-25', 1),
(2, 'Bob Smith', 30, 2, '2023-11-26', 2),
(3, 'Eva Brown', 45, 3, '2023-11-27', NULL),
(4, 'Jack Wilson', 50, 4, '2023-11-28', 1),
(5, 'Sophia Davis', 28, 5, '2023-11-29', 2),
(6, 'Noah Miller', 35, 6, '2023-11-30', NULL),
(7, 'Olivia Martinez', 22, 7, '2023-12-01', 1),
(8, 'Liam Garcia', 40, 8, '2023-12-02', 2),
(9, 'Ava Rodriguez', 27, 9, '2023-12-03', NULL),
(10, 'Mia Hernandez', 55, 10, '2023-12-04', 1);

mysql> desc Passengers;
+---------------+--------------+------+-----+---------+----------------+
| Field         | Type         | Null | Key | Default | Extra          |
+---------------+--------------+------+-----+---------+----------------+
| PassengerID   | int          | NO   | PRI | NULL    | auto_increment |
| Name          | varchar(100) | YES  |     | NULL    |                |
| Age           | int          | YES  |     | NULL    |                |
| RouteID       | int          | YES  | MUL | NULL    |                |
| BookingDate   | date         | YES  |     | NULL    |                |
| TravelAgentID | int          | YES  | MUL | NULL    |                |
+---------------+--------------+------+-----+---------+----------------+

 mysql> select* from Passengers;
+-------------+-----------------+------+---------+-------------+---------------+
| PassengerID | Name            | Age  | RouteID | BookingDate | TravelAgentID |
+-------------+-----------------+------+---------+-------------+---------------+
|           1 | Alice Johnson   |   25 |       1 | 2023-11-25  |             1 |
|           2 | Bob Smith       |   30 |       2 | 2023-11-26  |             2 |
|           3 | Eva Brown       |   45 |       3 | 2023-11-27  |          NULL |
|           4 | Jack Wilson     |   50 |       4 | 2023-11-28  |             1 |
|           5 | Sophia Davis    |   28 |       5 | 2023-11-29  |             2 |
|           6 | Noah Miller     |   35 |       6 | 2023-11-30  |          NULL |
|           7 | Olivia Martinez |   22 |       7 | 2023-12-01  |             1 |
|           8 | Liam Garcia     |   40 |       8 | 2023-12-02  |             2 |
|           9 | Ava Rodriguez   |   27 |       9 | 2023-12-03  |          NULL |
|          10 | Mia Hernandez   |   55 |      10 | 2023-12-04  |             1 |
+-------------+-----------------+------+---------+-------------+---------------+


mysql> 
mysql>
mysql>
mysql>
mysql>
mysql>
mysql>
mysql>
mysql>

