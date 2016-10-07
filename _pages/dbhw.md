# DB Homwork 1

```sql
CREATE TABLE student(
	UNI text primary key,
	name text not null,
	age int not null,
	res_field text,
	concentration text,
	dept_name references department not null,
	CHECK( 
		(res_field is not null or concentration is not null)
		and (age > 18)
		)
);
```

```sql
CREATE TABLE department(
	dept_name text primary key,
	address text
);
```

```sql
CREATE TABLE course(
	Course_num int not null,
	Capacity int not null,
	name text not null,
	dept_name references department,
	PRIMARY KEY(Course_num, department)
);
```

```sql
CREATE TABLE term(
	semester text not null,
	year int not null,
	PRIMARY KEY(semester, year)
);
```
```sql
CREATE TABLE Registration(
	semester references term,
	year references term,
	Course_num references course,
	dept_name references department,
	UNI references student,
	PRIMARY KEY(UNI, Course_num, dept_name, year, semester)
);
```
