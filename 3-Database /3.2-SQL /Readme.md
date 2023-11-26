CREATE TABLE Students (
  StudentID INT PRIMARY KEY,
  StudTicket VARCHAR(10) UNIQUE,
  Name VARCHAR(50) NOT NULL,
  Surname VARCHAR(50) NOT NULL,
  FatherName VARCHAR(50),
  Faculty VARCHAR(255) NOT NULL,
  Course INT CHECK (Course >= 1 AND Course <= 6),
  GroupName VARCHAR(8)
);

CREATE TABLE Electors (
    ElectorID INT PRIMARY KEY,
    StudentID INT NOT NULL,
    accessCode VARCHAR(10) NOT NULL,
    isAlreadyVote BOOLEAN,
    isCandidate BOOLEAN,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);

CREATE TABLE Candidates (
    CandidateID INT PRIMARY KEY,
    StudentID INT NOT NULL,
    ElectionProgram TEXT,
    isRegistred BOOLEAN,
    isActive BOOLEAN,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);

CREATE TABLE Administrators (
    AdministratorID INT PRIMARY KEY,
    StudentID INT NOT NULL,
    specialCode INT NOT NULL,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);

CREATE TABLE Results (
    ResultID INT PRIMARY KEY,
    CandidateID INT NOT NULL,
    TotalVotesCount INT NOT NULL,
    FOREIGN KEY (CandidateID) REFERENCES Candidates(CandidateID)
);
