Complete University Scholarship Allocation System for you - with all 6 parts you asked for.1. PROBLEM STATEMENTTitle: University Scholarship Allocation SystemObjective: To design and implement an automated system that evaluates student applications for scholarships based on multiple eligibility criteria and ranks them fairly.Eligibility Criteria:Aggregate Marks: Student must have aggregate >= 60%Subject-wise Minimum: Each subject score must be >= 35 marksAttendance: Minimum 75% attendance requiredAnnual Family Income: Must be <= Rs. 2,00,000Ranking & Tie-Breaking Rules:Primary Rank: Higher aggregate marks gets higher rankTie-Breaker 1: If aggregate is equal, higher attendance is preferredTie-Breaker 2: If still tied, lower family income is preferredTie-Breaker 3: If still tied, higher Math marks is preferredExpected Outcome: List of eligible students ranked for scholarship and list of rejected students with reason.2. ALGORITHMjavascriptAlgorithm: Scholarship Allocation
Input: List of Applicants with marks, attendance, income
Output: Ranked list of eligible students

Step 1: Start
Step 2: Define constants
       AGG_MIN = 60, SUB_MIN = 35, ATT_MIN = 75, INCOME_MAX = 200000
Step 3: For each applicant in applicants list:
        Step 3.1: Check aggregate < AGG_MIN -> REJECT (Low Aggregate)
        Step 3.2: Check any subject < SUB_MIN -> REJECT (Subject Backlog)
        Step 3.3: Check attendance < ATT_MIN -> REJECT (Low Attendance)
        Step 3.4: Check income > INCOME_MAX -> REJECT (Income High)
        Step 3.5: Else -> Mark as ELIGIBLE
Step 4: If eligible list is empty -> Print "No eligible students" and Stop
Step 5: Sort eligible list by:
        - aggregate DESC
        - attendance DESC
        - income ASC
        - math_marks DESC
Step 6: Assign Rank 1, 2, 3... to sorted list
Step 7: Display Ranked Eligible List and Rejected List with Reasons
Step 8: Stop3. FLOWCHART 
    A[Start] --> B[Input Applicants Data]
    B --> C{For Each Applicant}
    C --> D{Aggregate >= 60%?}
    D -- No --> E[Reject: Low Aggregate]
    D -- Yes --> F{All Subjects >= 35?}
    F -- No --> G[Reject: Subject Fail]
    F -- Yes --> H{Attendance >= 75%?}
    H -- No --> I[Reject: Low Attendance]
    H -- Yes --> J{Income <= 2 Lakhs?}
    J -- No --> K[Reject: High Income]
    J -- Yes --> L[Mark as Eligible]
    E & G & I & K --> C
    L --> C
    C -- All Checked --> M{Eligible List Empty?}
    M -- Yes --> N[No Scholarship Awarded]
    M -- No --> O[Sort Eligible List]
    O --> P[Sort by: 1. Aggregate DESC, 2. Attendance DESC, 3. Income ASC, 4. Math DESC]
    P --> Q[Assign Ranks]
    Q --> R[Display Ranked List & Rejected List]
    R --> S[End]
    N --> S4. SOURCE CODE (Python)pythonclass Applicant:
    def __init__(self, id, name, subjects, attendance, income):
        self.id = id
        self.name = name
        self.subjects = subjects  # dict e.g. {'Math': 85, 'Physics': 78...}
        self.attendance = attendance
        self.income = income
        self.aggregate = sum(subjects.values()) / len(subjects)
        self.status = "PENDING"
        self.reason = ""

    def __repr__(self):
        return f"{self.name} - {self.aggregate:.2f}%"

def evaluate_applicants(applicants):
    ELIGIBLE = []
    REJECTED = []
    
    # Criteria Constants
    AGG_MIN = 60
    SUB_MIN = 35
    ATT_MIN = 75
    INCOME_MAX = 200000

    for app in applicants:
        # Check 1: Aggregate
        if app.aggregate < AGG_MIN:
            app.status = "REJECTED"
            app.reason = f"Aggregate {app.aggregate:.1f}% < {AGG_MIN}%"
            REJECTED.append(app)
            continue
        
        # Check 2: Subject-wise
        failed_sub = [s for s, m in app.subjects.items() if m < SUB_MIN]
        if failed_sub:
            app.status = "REJECTED"
            app.reason = f"Failed in {', '.join(failed_sub)} (< {SUB_MIN})"
            REJECTED.append(app)
            continue

        # Check 3: Attendance
        if app.attendance < ATT_MIN:
            app.status = "REJECTED"
            app.reason = f"Attendance {app.attendance}% < {ATT_MIN}%"
            REJECTED.append(app)
            continue

        # Check 4: Income
        if app.income > INCOME_MAX:
            app.status = "REJECTED"
            app.reason = f"Income Rs.{app.income} > {INCOME_MAX}"
            REJECTED.append(app)
            continue

        # Eligible
        app.status = "ELIGIBLE"
        ELIGIBLE.append(app)

    # Ranking with Tie-Breaking Rules
    ELIGIBLE.sort(key=lambda x: (-x.aggregate, -x.attendance, x.income, -x.subjects.get('Math', 0)))
    
    return ELIGIBLE, REJECTED

# ------------------- MAIN DRIVER -------------------
if __name__ == "__main__":
    # Sample Data - You can change this
    applicants = [
        Applicant("S01", "Aarav Sharma", {'Math': 92, 'Physics': 88, 'Chemistry': 85}, 88, 150000),
        Applicant("S02", "Priya Patel", {'Math': 85, 'Physics': 85, 'Chemistry': 85}, 92, 120000),
        Applicant("S03", "Rohan Verma", {'Math': 55, 'Physics': 58, 'Chemistry': 60}, 80, 100000), # Low aggregate
        Applicant("S04", "Ananya Singh", {'Math': 78, 'Physics': 30, 'Chemistry': 82}, 85, 180000), # Subject fail
        Applicant("S05", "Vikram Rao", {'Math': 85, 'Physics': 85, 'Chemistry': 85}, 78, 90000),  # Tie with Priya
        Applicant("S06", "Sneha Reddy", {'Math': 90, 'Physics': 89, 'Chemistry': 91}, 70, 110000), # Low attendance
        Applicant("S07", "Karan Mehta", {'Math': 88, 'Physics': 82, 'Chemistry': 80}, 90, 250000), # High income
        Applicant("S08", "Isha Gupta", {'Math': 75, 'Physics': 76, 'Chemistry': 74}, 82, 130000),
    ]

    eligible, rejected = evaluate_applicants(applicants)

    print("="*60)
    print("SCHOLARSHIP RANK LIST")
    print("="*60)
    if not eligible:
        print("No eligible students found.")
    else:
        print(f"{'Rank':<5} {'ID':<5} {'Name':<15} {'Agg%':<7} {'Att%':<6} {'Income':<10}")
        print("-"*60)
        for rank, student in enumerate(eligible, 1):
            print(f"{rank:<5} {student.id:<5} {student.name:<15} {student.aggregate:<7.1f} {student.attendance:<6} {student.income:<10}")

    print("\n" + "="*60)
    print("REJECTED LIST WITH REASONS")
    print("="*60)
    for student in rejected:
        print(f"{student.id} - {student.name}: {student.reason}")5. SOURCE INPUTThe input is defined in the applicants list in the main driver:IDNameMathPhysicsChemistryAttendanceIncomeS01Aarav Sharma92888588%150000S02Priya Patel85858592%120000S03Rohan Verma55586080%100000S04Ananya Singh78308285%180000S05Vikram Rao85858578%90000S06Sneha Reddy90899170%110000S07Karan Mehta88828090%250000S08Isha Gupta75767482%1300006. SOURCE OUTPUTWhen you run the code, you get:javascript
SCHOLARSHIP RANK LIST

Rank  ID    Name            Agg%    Att%   Income    

1     S01   Aarav Sharma    88.3    88     150000    
2     S02   Priya Patel     85.0    92     120000    
3     S05   Vikram Rao      85.0    78     90000     
4     S08   Isha Gupta      75.0    82     130000    

REJECTED LIST WITH REASONS

S03 - Rohan Verma: Aggregate 57.7% < 60%
S04 - Ananya Singh: Failed in Physics (< 35)
S06 - Sneha Reddy: Attendance 70% < 75%
S07 - Karan Mehta: Income Rs.250000 > 200000Note on Tie Resolution: See Priya Patel (S02) and Vikram Rao (S05) - both have 85% aggregate. Tie was resolved by Attendance (92% > 78%), so Priya got Rank 2.Want me to generate this as a .py file and a PDF report for your submission?In this you have to write algorithm,flowchart,python sorce code,sample input,sample output,and the flow chart in mermaid
