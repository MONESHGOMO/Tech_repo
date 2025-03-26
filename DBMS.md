# Database Lab Application - Schema & Relationships

## 1. Overview
This document describes the **Entity-Relationship (ER) Model** for a Database Lab Application. It includes how tables are created, their attributes, and the relationships between them.

## 2. Database Schema
The following tables are created in the database:

### **1. Student Table**
Holds information about students enrolled in the lab.
```sql
CREATE TABLE Student (
    Student_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100) UNIQUE,
    Department VARCHAR(50),
    Year INT,
    Roll_Number VARCHAR(20) UNIQUE
);
```
#### **Attributes:**
- **Student_ID**: Unique identifier for each student (Primary Key).
- **Name**: Student's full name.
- **Email**: Unique email for each student.
- **Department**: Student's department.
- **Year**: Year of study.
- **Roll_Number**: Unique roll number.

---

### **2. Instructor Table**
Stores details of instructors conducting the lab.
```sql
CREATE TABLE Instructor (
    Instructor_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100) UNIQUE,
    Department VARCHAR(50)
);
```
#### **Attributes:**
- **Instructor_ID**: Unique identifier (Primary Key).
- **Name**: Instructor's name.
- **Email**: Unique instructor email.
- **Department**: Department associated with the instructor.

---

### **3. Course Table**
Stores course details along with the instructor responsible.
```sql
CREATE TABLE Course (
    Course_ID INT PRIMARY KEY,
    Course_Name VARCHAR(100),
    Instructor_ID INT,
    FOREIGN KEY (Instructor_ID) REFERENCES Instructor(Instructor_ID)
);
```
#### **Relationships:**
- **Instructor_ID** is a Foreign Key linking to **Instructor(Instructor_ID)**.

---

### **4. Lab Table**
Contains information about lab locations and capacity.
```sql
CREATE TABLE Lab (
    Lab_ID INT PRIMARY KEY,
    Lab_Name VARCHAR(100),
    Location VARCHAR(100),
    Capacity INT
);
```
#### **Attributes:**
- **Lab_ID**: Unique identifier (Primary Key).
- **Lab_Name**: Name of the lab.
- **Location**: Where the lab is located.
- **Capacity**: Maximum number of students it can accommodate.

---

### **5. Lab_Experiment Table**
Stores details of experiments conducted in labs.
```sql
CREATE TABLE Lab_Experiment (
    Experiment_ID INT PRIMARY KEY,
    Experiment_Name VARCHAR(100),
    Description TEXT,
    Lab_ID INT,
    FOREIGN KEY (Lab_ID) REFERENCES Lab(Lab_ID)
);
```
#### **Relationships:**
- **Lab_ID** is a Foreign Key linking to **Lab(Lab_ID)**.

---

### **6. Assignment_Submission Table**
Tracks assignment submissions by students for lab experiments.
```sql
CREATE TABLE Assignment_Submission (
    Submission_ID INT PRIMARY KEY,
    Student_ID INT,
    Experiment_ID INT,
    Submission_Date DATE,
    Grade CHAR(2),
    FOREIGN KEY (Student_ID) REFERENCES Student(Student_ID),
    FOREIGN KEY (Experiment_ID) REFERENCES Lab_Experiment(Experiment_ID)
);
```
#### **Relationships:**
- **Student_ID** is a Foreign Key linking to **Student(Student_ID)**.
- **Experiment_ID** is a Foreign Key linking to **Lab_Experiment(Experiment_ID)**.

---

## 3. Data Mapping and Relationships

### **ER Model Relationships:**
1. **Instructor → Course** (One-to-Many): One instructor can handle multiple courses.
2. **Lab → Lab_Experiment** (One-to-Many): Each lab has multiple experiments.
3. **Student → Assignment_Submission** (One-to-Many): Each student submits multiple assignments.
4. **Lab_Experiment → Assignment_Submission** (One-to-Many): Each experiment can have multiple student submissions.

### **Example Queries**

#### **Insert Data into Tables**
```sql
INSERT INTO Student (Student_ID, Name, Email, Department, Year, Roll_Number) 
VALUES (1, 'Alice Johnson', 'alice@example.com', 'CSE', 2, 'CSE2021001');

INSERT INTO Instructor (Instructor_ID, Name, Email, Department) 
VALUES (1, 'Dr. Williams', 'williams@example.com', 'CSE');

INSERT INTO Course (Course_ID, Course_Name, Instructor_ID) 
VALUES (101, 'Database Systems', 1);

INSERT INTO Lab (Lab_ID, Lab_Name, Location, Capacity) 
VALUES (1, 'DBMS Lab', 'Room 305', 30);

INSERT INTO Lab_Experiment (Experiment_ID, Experiment_Name, Description, Lab_ID) 
VALUES (1, 'Normalization', 'Study of Normal Forms', 1);

INSERT INTO Assignment_Submission (Submission_ID, Student_ID, Experiment_ID, Submission_Date, Grade) 
VALUES (1, 1, 1, '2024-03-25', 'A');
```

#### **Retrieve Student Submissions with Experiment Details**
```sql
SELECT S.Student_ID, S.Name, E.Experiment_Name, A.Grade 
FROM Student S
JOIN Assignment_Submission A ON S.Student_ID = A.Student_ID
JOIN Lab_Experiment E ON A.Experiment_ID = E.Experiment_ID
ORDER BY A.Grade;
```

---

## 4. Summary
- The **Database Lab Application** consists of students, instructors, courses, labs, and experiments.
- **Foreign Key constraints** ensure relational integrity.
- The schema allows tracking **student submissions and grades**.
- The design supports **future extensions** like additional labs, instructors, and students.

---

# Spring Boot JPA - Entity Mapping for Database Lab Application

## Overview
This document explains how entity relationships are mapped using **Spring Boot, Spring Data JPA, and Hibernate**. It includes entity classes, relationships, and annotations for database mapping.

## 1. Student Entity
Defines the **Student** table and maps it using JPA.

```java
import jakarta.persistence.*;
import lombok.Data;
import java.util.Set;

@Entity
@Data
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long studentId;
    
    private String name;
    private String email;
    private String department;
    private int year;
    private String rollNumber;
    
    @OneToMany(mappedBy = "student", cascade = CascadeType.ALL)
    private Set<AssignmentSubmission> submissions;
}
```

## 2. Instructor Entity
Defines the **Instructor** table and its mapping.

```java
import jakarta.persistence.*;
import lombok.Data;
import java.util.Set;

@Entity
@Data
public class Instructor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long instructorId;
    
    private String name;
    private String email;
    private String department;
    
    @OneToMany(mappedBy = "instructor", cascade = CascadeType.ALL)
    private Set<Course> courses;
}
```

## 3. Course Entity
Defines **Course** and links it to **Instructor**.

```java
import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long courseId;
    
    private String courseName;
    
    @ManyToOne
    @JoinColumn(name = "instructor_id", nullable = false)
    private Instructor instructor;
}
```

## 4. Lab Entity
Defines **Lab** and its mapping.

```java
import jakarta.persistence.*;
import lombok.Data;
import java.util.Set;

@Entity
@Data
public class Lab {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long labId;
    
    private String labName;
    private String location;
    private int capacity;
    
    @OneToMany(mappedBy = "lab", cascade = CascadeType.ALL)
    private Set<LabExperiment> experiments;
}
```

## 5. Lab Experiment Entity
Defines **Lab Experiment** and links it to **Lab**.

```java
import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class LabExperiment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long experimentId;
    
    private String experimentName;
    private String description;
    
    @ManyToOne
    @JoinColumn(name = "lab_id", nullable = false)
    private Lab lab;
}
```

## 6. Assignment Submission Entity
Defines **Assignment Submission** and links it to **Student** and **Lab Experiment**.

```java
import jakarta.persistence.*;
import lombok.Data;
import java.util.Date;

@Entity
@Data
public class AssignmentSubmission {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long submissionId;
    
    @ManyToOne
    @JoinColumn(name = "student_id", nullable = false)
    private Student student;
    
    @ManyToOne
    @JoinColumn(name = "experiment_id", nullable = false)
    private LabExperiment experiment;
    
    private Date submissionDate;
    private String grade;
}
```

## Summary
- **One-to-Many Relationship:** 
  - `Instructor` → `Course`
  - `Lab` → `LabExperiment`
  - `Student` → `AssignmentSubmission`
- **Many-to-One Relationship:**
  - `Course` → `Instructor`
  - `LabExperiment` → `Lab`
  - `AssignmentSubmission` → `Student` & `LabExperiment`

This setup ensures proper entity mapping using **Spring Boot JPA** and **Hibernate**.

---
**Author: Gomo**  
**Date: March 27, 2025**
