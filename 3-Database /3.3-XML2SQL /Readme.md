DECLARE @DocHandle INT;
DECLARE @XmlDocument NVARCHAR(MAX);

SET @XmlDocument = N'<KNUBA>
<Students>
  <Student>
    <StudentID>1</StudentID>
    <StudTicket>КВ12345678</StudTicket>
    <Name>Данило</Name>
    <Surname>Гончарук</Surname>
    <FatherName>Олексійович</FatherName>
    <Faculty>ФАІТ</Faculty>
    <Course>5</Course>
    <GroupName>ІПЗм-23</GroupName>
  </Student>
  <Student>
    <StudentID>2</StudentID>
    <StudTicket>КВ12345679</StudTicket>
    <Name>Віктор</Name>
    <Surname>Іваеченко</Surname>
    <FatherName>Володимирович</FatherName>
    <Faculty>ФАІТ</Faculty>
    <Course>6</Course>
    <GroupName>ІПЗм-22</GroupName>
  </Student>
  <Student>
    <StudentID>3</StudentID>
    <StudTicket>КВ12344679</StudTicket>
    <Name>Олена</Name>
    <Surname>Петренко</Surname>
    <FatherName>Андріївна</FatherName>
    <Faculty>ФАІТ</Faculty>
    <Course>3</Course>
    <GroupName>КНб-21</GroupName>
  </Student>
</Students>
<Electors>
  <Elector>
    <ElectorID>1</ElectorID>
    <StudentID>1</StudentID>
    <AccessCode>Afa912afk0</AccessCode>
    <isAlreadyVote>0</isAlreadyVote>
    <isCandidate>1</isCandidate>
  </Elector>
  <Elector>
    <ElectorID>2</ElectorID>
    <StudentID>2</StudentID>
    <AccessCode>Afa992aPk0</AccessCode>
    <isAlreadyVote>0</isAlreadyVote>
    <isCandidate>1</isCandidate>
  </Elector>
  <Elector>
    <ElectorID>3</ElectorID>
    <StudentID>3</StudentID>
    <AccessCode>KKd9961Po0</AccessCode>
    <isAlreadyVote>0</isAlreadyVote>
    <isCandidate>0</isCandidate>
  </Elector>
</Electors>
<Candidates>
    <Candidate>
        <CandidateID>1</CandidateID>
        <StudentID>1</StudentID>
        <ElectionProgram>Виборча програма 1</ElectionProgram>
        <isRegistred>1</isRegistred>
        <isActive>1</isActive>
    </Candidate>
    <Candidate>
        <CandidateID>2</CandidateID>
        <StudentID>2</StudentID>
        <ElectionProgram>Виборча програма 2</ElectionProgram>
        <isRegistred>1</isRegistred>
        <isActive>1</isActive>
    </Candidate>
</Candidates>
<Administrators>
    <Administrator>
        <AdministratorID>1</AdministratorID>
        <StudentID>3</StudentID>
        <specialCode>1234567890</specialCode>
    </Administrator>
</Administrators>
<Results>
    <Result>
        <ResultID>1</ResultID>
        <CandidateID>1</CandidateID>
        <TotalVotesCount>0</TotalVotesCount>
    </Result>
    <Result>
        <ResultID>2</ResultID>
        <CandidateID>2</CandidateID>
        <TotalVotesCount>0</TotalVotesCount>
    </Result>
</Results>
</KNUBA>';


EXEC sp_xml_preparedocument @DocHandle OUTPUT, @XmlDocument;

-- Вставка даних з таблиці Students
INSERT INTO Students
(
    StudentID,
    StudTicket,
    Name,
    Surname,
    FatherName,
    Faculty,
    Course,
    GroupName
)
SELECT 
    StudentID,
    StudTicket,
    Name,
    Surname,
    FatherName,
    Faculty,
    Course,
    GroupName
FROM OPENXML(@DocHandle, '/KNUBA/Students/Student', 2)
WITH
(
    StudentID INT 'StudentID',
    StudTicket VARCHAR(10) 'StudTicket',
    Name VARCHAR(50) 'Name',
    Surname VARCHAR(50) 'Surname',
    FatherName VARCHAR(50) 'FatherName',
    Faculty VARCHAR(50) 'Faculty',
    Course INT 'Course',
    GroupName VARCHAR(8) 'GroupName'
);

-- Вставка даних з таблиці Electors
INSERT INTO Electors
(
    ElectorID,
    StudentID,
    AccessCode,
    isAlreadyVote,
    isCandidate
)
SELECT 
    ElectorID,
    StudentID,
    AccessCode,
    isAlreadyVote,
    isCandidate
FROM OPENXML(@DocHandle, '/KNUBA/Electors/Elector', 2)
WITH
(
    ElectorID INT 'ElectorID',
    StudentID INT 'StudentID',
    AccessCode VARCHAR(10) 'AccessCode',
    isAlreadyVote BIT 'isAlreadyVote',
    isCandidate BIT 'isCandidate'
);

-- Вставка даних з таблиці Candidates
INSERT INTO Candidates
(
    CandidateID,
    StudentID,
    ElectionProgram,
    isRegistred,
    isActive
)
SELECT 
    CandidateID,
    StudentID,
    ElectionProgram,
    isRegistred,
    isActive
FROM OPENXML(@DocHandle, '/KNUBA/Candidates/Candidate', 2)
WITH
(
    CandidateID INT 'CandidateID',
    StudentID INT 'StudentID',
    ElectionProgram VARCHAR(MAX) 'ElectionProgram',
    isRegistred BIT 'isRegistred',
    isActive BIT 'isActive'
);

-- Вставка даних з таблиці Administrators
INSERT INTO Administrators
(
    AdministratorID,
    StudentID,
    specialCode
)
SELECT 
    AdministratorID,
    StudentID,
    specialCode
FROM OPENXML(@DocHandle, '/KNUBA/Administrators/Administrator', 2)
WITH
(
    AdministratorID INT 'AdministratorID',
    StudentID INT 'StudentID',
    specialCode INT 'specialCode'
);


-- Вставка даних з таблиці Results
INSERT INTO Results
(
    ResultID,
    CandidateID,
    TotalVotesCount
)
SELECT 
    ResultID,
    CandidateID,
    TotalVotesCount
FROM OPENXML(@DocHandle, '/KNUBA/Results/Result', 2)
WITH
(
    ResultID INT 'ResultID',
    CandidateID INT 'CandidateID',
    TotalVotesCount INT 'TotalVotesCount'
);

EXEC sp_xml_removedocument @DocHandle;
