| Об’єкт |	Атрибут |	Короткий опис	| Тип	| Обмеження |
|-------:|-------:|-------:|-------:|-------:|
| Elector |	studID |	Ідентифікатор студента, серія та номер його студ. квитка	| String	| Not Null, Unique |
| Elector |	accessCode |	Код доступу, приходить на пошту загальною розсилкою через засіб комунікацій університету |	String |	Not Null |
| Elector |	name	| Ім’я студента	| String |	Not Null |
| Elector |	surname |	Прізвище студента	| String	| Not Null |
| Elector |	fatherName |	По-батькові студента |	String |	Default Null |
| Elector |	faculty |	Факультет, на якому навчається студент |	String	| Not Null |
| Elector |	course |	Курс, на якому навчається студент	| Int	| Not Null, 1 =< coruse =< 6 |
| Elector |	group |	Група, у якій навчається студент	| String	| Not Null |
| Elector |	isAlreadyVote |	Позначення, чи голосував уже студент на поточних виборах |	Boolean |	Default = false |
| Candidate	| studID |	Ідентифікатор студента, серія та номер його студ. квитка	| String	| Not Null, Unique |
| Candidate	| 	accessCode |	Код доступу, приходить на пошту загальною розсилкою через засіб комунікацій університету |	String |	Not Null |
| Candidate	| 	name |	Ім’я студента |	String	| Not Null |
| Candidate	| 	surname |	Прізвище студента	| String	| Not Null |
| Candidate	| 	fatherName |	По-батькові студента |	String |	Default Null |
| Candidate	| 	faculty |	Факультет, на якому навчається студент |	String |	Not Null |
| Candidate	| 	course |	Курс, на якому навчається студент	| Int	| Not Null, 1 =< coruse =< 6 |
| Candidate	| 	group |	Група, у якій навчається студент |	String	| Not Null |
| Candidate	| 	isAlreadyVote |	Позначення, чи голосував уже студент на поточних виборах |	Boolean |	Default = false |
| Candidate	| 	isRegistrated |	Позначення, чи прийняли кандидатуру студента на пост голови студ. ради |	Boolean	| Default = false |
| Administrator |	studID |	Ідентифікатор адміністратора, встановлюється безпосередньо у базі даних	| String |	Not Null, Unique |
| Administrator |	accessCode |	Код доступу, приходить на пошту загальною розсилкою через засіб комунікацій університету	| String |	Not Null |
| Administrator |	name	| Ім’я адміністратора	| String	| Not Null |
| Administrator |	surname |	Прізвище адміністратора	| String	| Not Null |
| Administrator |	fatherName |	По-батькові адміністратора |	String |	Default Null |
| Administrator |	specialCode |	Спеціальний код для управління виборчим процесом, встановлюється безпосередньо у базі даних |	Int |	Not Null |
