# JOBSHEET 6 - SELECTION STATEMENTS 2

![Java](https://img.shields.io/badge/Java-21-orange?logo=openjdk&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Selection_2-blue)
![Jobsheet](https://img.shields.io/badge/Jobsheet-6-success)
![Class](https://img.shields.io/badge/Class-TI__1I-purple)

## Student Identity

- **Name:** Raul Ibrahim Hernov
- **NIM:** 264107020258
- **Study Program:** D-IV Informatics Engineering
- **Class:** TI_1I

---

## 1. OBJECTIVE

The practical work in this chapter focuses on:

1. Solving problems and case studies using nested selection statements.
2. Applying nested selection statements (Nested IF) in Java programs.
3. Applying the logical operators `&&` (AND), `||` (OR), and `!` (NOT) in selection structures.

> [!NOTE]
> Every output shown in this report comes from an actual run of the program.

---

## 2. LABS & ACTIVITIES

### 2.1 Experiment 1: Nested IF to Check Thesis Exam Requirements

A student wants to register for the thesis exam. The SIMTA system first checks an administrative requirement: the student must have no outstanding penalties. If this requirement is met, the system then checks the guidance log. To register, the student must have at least 8 guidance sessions with Supervisor 1 and at least 4 guidance sessions with Supervisor 2. If all requirements are met, the student can proceed to register for the thesis exam. If not, the system shows the reason for failure.

> [!NOTE]
> This is a nested selection: the guidance log is only checked when the penalty requirement is met.

#### 2.1.1 Program Code Java

```java
package jobsheet6;
import java.util.Scanner;

public class NestedThesisExamAttendanceNo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String message;

        System.out.print("Has the student cleared all penalties? (Yes/No): ");
        String noPenalty = sc.nextLine().trim();

        System.out.print("Enter the number of guidance sessions with Supervisor 1: ");
        int guidanceCount1 = sc.nextInt();
        System.out.print("Enter the number of guidance sessions with Supervisor 2: ");
        int guidanceCount2 = sc.nextInt();

        if (noPenalty.equalsIgnoreCase("Yes")) {
            if (guidanceCount1 >= 8 && guidanceCount2 >= 4) {
                message = "All requirements met. The student may register for the thesis exam";
            } else if (guidanceCount1 < 8 && guidanceCount2 < 4) {
                message = "Failed! Guidance sessions with Supervisor 1 are below 8 and Supervisor 2 are below 4";
            } else if (guidanceCount1 < 8) {
                message = "Failed! Guidance sessions with Supervisor 1 have not reached 8";
            } else {
                message = "Failed! Guidance sessions with Supervisor 2 have not reached 4";
            }
        } else {
            message = "Failed! The student still has an outstanding penalty";
        }
        System.out.println(message);
        sc.close();
    }
}
```

> [!TIP]
> `trim()` removes leading and trailing spaces from the input, and `equalsIgnoreCase("Yes")` makes `yes`, `YES`, and `Yes` all accepted.

#### 2.1.2 Execution Result / Screenshot Output

```diff
  Has the student cleared all penalties? (Yes/No): yes
  Enter the number of guidance sessions with Supervisor 1: 6
  Enter the number of guidance sessions with Supervisor 2: 5
- Failed! Guidance sessions with Supervisor 1 have not reached 8
```

The program is run with the input from the jobsheet. The penalty check passes (`yes`), but Supervisor 1 has only 6 sessions (below 8), so the program prints the failure message for Supervisor 1.

> [!WARNING]
> The guidance counts are read with `sc.nextInt()`. Entering text instead of a number throws an `InputMismatchException`.

#### 2.1.3 Answers to Questions / Reflection Questions

$\color{#0969da}{\textbf{Question 1:}}$ **What happens if the student answers "No" to the penalty-clearance question? Why?**

$\color{#1a7f37}{\textbf{Answer:}}$ The program prints `Failed! The student still has an outstanding penalty`. The first-level condition `noPenalty.equalsIgnoreCase("Yes")` becomes `false`, so the program skips the whole inner IF block (the guidance session checks) and jumps directly to the outer `else`. This is the idea of a nested IF: the second-level check only runs when the first-level condition is true. Any input other than "Yes" (for example "No" or "Y") is treated the same way.

```diff
  Has the student cleared all penalties? (Yes/No): No
  Enter the number of guidance sessions with Supervisor 1: 10
  Enter the number of guidance sessions with Supervisor 2: 5
- Failed! The student still has an outstanding penalty
```

Even though the guidance sessions (10 and 5) are enough, the student still fails because the administrative requirement is checked first. In this program the two guidance numbers are still asked, because the input is read before the IF structure; they are just never evaluated.

> [!IMPORTANT]
> In a nested IF, the inner condition is evaluated only when the outer condition is true.

$\color{#0969da}{\textbf{Question 2:}}$ **Explain the meaning of the following code snippet!**

```java
if (guidanceCount1 >= 8 && guidanceCount2 >= 4) {
```

$\color{#1a7f37}{\textbf{Answer:}}$ The condition checks two things at once using the AND operator (`&&`):

- `guidanceCount1 >= 8` is true if the student has at least 8 guidance sessions with Supervisor 1.
- `guidanceCount2 >= 4` is true if the student has at least 4 guidance sessions with Supervisor 2.

The whole condition is true only if both parts are true. If one part (or both) is false, the condition is false and the program moves on to the next `else if` / `else`. So this line means: "the student meets the guidance requirement with both supervisors."

$\color{#0969da}{\textbf{Question 3:}}$ **Describe the full flow of checking the student's requirements from start to finish. Explain step by step for every condition!**

$\color{#1a7f37}{\textbf{Answer:}}$

1. The program reads the penalty status (`noPenalty`) and the number of guidance sessions with Supervisor 1 (`guidanceCount1`) and Supervisor 2 (`guidanceCount2`).
2. **First level:** `noPenalty.equalsIgnoreCase("Yes")`.
   - If false, the program prints `Failed! The student still has an outstanding penalty` and finishes.
   - If true, the program enters the second level.
3. **Second level**, checked in this order:
   1. `guidanceCount1 >= 8 && guidanceCount2 >= 4`: both guidance requirements are met, so the message is `All requirements met. The student may register for the thesis exam`.
   2. `guidanceCount1 < 8 && guidanceCount2 < 4`: both are not enough, so the message is `Failed! Guidance sessions with Supervisor 1 are below 8 and Supervisor 2 are below 4`.
   3. `guidanceCount1 < 8`: at this point Supervisor 2 must already be fine (since the previous conditions were false), so only Supervisor 1 is not enough. The message is `Failed! Guidance sessions with Supervisor 1 have not reached 8`.
   4. `else`: the only remaining possibility is that Supervisor 1 is fine but Supervisor 2 is below 4. The message is `Failed! Guidance sessions with Supervisor 2 have not reached 4`.
4. The program prints the value of `message` with `System.out.println(message)`.

Test results for every possible output:

| Penalty cleared | Supervisor 1 | Supervisor 2 | Output |
|---|---|---|---|
| Yes | 8 | 4 | All requirements met. The student may register for the thesis exam |
| Yes | 5 | 2 | Failed! Guidance sessions with Supervisor 1 are below 8 and Supervisor 2 are below 4 |
| Yes | 6 | 5 | Failed! Guidance sessions with Supervisor 1 have not reached 8 |
| Yes | 9 | 3 | Failed! Guidance sessions with Supervisor 2 have not reached 4 |
| No | 10 | 5 | Failed! The student still has an outstanding penalty |

---

### 2.2 Experiment 2: Logical Operators to Determine Campus WiFi Access

The campus WiFi can only be used by students or lecturers whose accounts are not blocked. The program receives information on whether the user is a student, whether the user is a lecturer, and whether the account is currently blocked. Access is granted if the user is a student or a lecturer, and the account is not blocked.

> [!TIP]
> `||` needs at least one true operand, `&&` needs all operands to be true, and `!` flips true to false and false to true.

#### 2.2.1 Program Code Java

```java
package jobsheet6;
import java.util.Scanner;

public class LogicalOperatorWifiAttendanceNo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        boolean isStudent;
        boolean isLecturer;
        boolean isBlocked;

        System.out.print("Is the user a student? (true/false): ");
        isStudent = sc.nextBoolean();
        System.out.print("Is the user a lecturer? (true/false): ");
        isLecturer = sc.nextBoolean();
        System.out.print("Is the account currently blocked? (true/false): ");
        isBlocked = sc.nextBoolean();

        if ((isStudent || isLecturer) && !isBlocked) {
            System.out.println("WiFi access granted");
        } else {
            System.out.println("WiFi access denied");
        }
        sc.close();
    }
}
```

#### 2.2.2 Execution Result / Screenshot Output

Test 1 (`true`, `false`, `false`):

```diff
  Is the user a student? (true/false): true
  Is the user a lecturer? (true/false): false
  Is the account currently blocked? (true/false): false
+ WiFi access granted
```

Test 2 (`false`, `true`, `false`):

```diff
  Is the user a student? (true/false): false
  Is the user a lecturer? (true/false): true
  Is the account currently blocked? (true/false): false
+ WiFi access granted
```

Test 3 (`true`, `false`, `true`):

```diff
  Is the user a student? (true/false): true
  Is the user a lecturer? (true/false): false
  Is the account currently blocked? (true/false): true
- WiFi access denied
```

Test 4 (`false`, `false`, `false`):

```diff
  Is the user a student? (true/false): false
  Is the user a lecturer? (true/false): false
  Is the account currently blocked? (true/false): false
- WiFi access denied
```

> [!WARNING]
> `sc.nextBoolean()` only accepts `true` or `false` (case-insensitive). Other input such as `yes` or `1` throws an `InputMismatchException`.

Summary of the test results:

| Test | isStudent | isLecturer | isBlocked | `(isStudent \|\| isLecturer) && !isBlocked` | Output |
|---|---|---|---|---|---|
| 1 | true | false | false | `true && true` = true | WiFi access granted |
| 2 | false | true | false | `true && true` = true | WiFi access granted |
| 3 | true | false | true | `true && false` = false | WiFi access denied |
| 4 | false | false | false | `false && true` = false | WiFi access denied |

#### 2.2.3 Answers to Questions / Reflection Questions

$\color{#0969da}{\textbf{Question 1:}}$ **Explain the function of the `||`, `&&`, and `!` operators in the condition above.**

$\color{#1a7f37}{\textbf{Answer:}}$

- `||` (OR): true if at least one operand is true. In `isStudent || isLecturer`, it is true if the user is a student, a lecturer, or both.
- `&&` (AND): true only if both operands are true. In `(isStudent || isLecturer) && !isBlocked`, the user must have a valid role and the account must not be blocked.
- `!` (NOT): reverses a boolean value. `!isBlocked` is true when `isBlocked` is false (the account is not blocked).

$\color{#0969da}{\textbf{Question 2:}}$ **Why can a lecturer still get access when `isStudent = false`?**

$\color{#1a7f37}{\textbf{Answer:}}$ Because the two roles are connected by `||`. The OR operator only needs one of its operands to be true, so when `isLecturer = true` the part `(isStudent || isLecturer)` is already true even though `isStudent` is false. If the account is not blocked, the whole condition is true and access is granted (Test 2).

$\color{#0969da}{\textbf{Question 3:}}$ **Change `||` to `&&`. Run the program again using test data 1 and 2. What happens, and why?**

$\color{#1a7f37}{\textbf{Answer:}}$ The condition becomes `(isStudent && isLecturer) && !isBlocked`.

```diff
- Test 1 (true, false, false) -> WiFi access denied
- Test 2 (false, true, false) -> WiFi access denied
```

Both tests are now denied, while before they were granted. With `&&`, the user must be a student and a lecturer at the same time. In test 1 `isLecturer` is false and in test 2 `isStudent` is false, so `isStudent && isLecturer` is false and the whole condition fails. This is why the correct operator for "student or lecturer" is `||`.

> [!CAUTION]
> Replacing `||` with `&&` is a logic error, not a compile error. The program still compiles and runs, but gives the wrong decision.

$\color{#0969da}{\textbf{Question 4:}}$ **In the expression `isStudent || isLecturer`, when does `isLecturer` not need to be evaluated? Explain using short-circuit evaluation.**

$\color{#1a7f37}{\textbf{Answer:}}$ When `isStudent` is `true`. With `||`, the result is already `true` if the left operand is `true`, no matter what the right operand is. Java uses short-circuit evaluation, so it stops there and skips `isLecturer`. The right operand is only evaluated when `isStudent` is `false`, because then the result depends on `isLecturer`.

$\color{#0969da}{\textbf{Question 5:}}$ **In the expression `(isStudent || isLecturer) && !isBlocked`, when does `!isBlocked` not need to be evaluated? Explain.**

$\color{#1a7f37}{\textbf{Answer:}}$ When `(isStudent || isLecturer)` is `false`, which means the user is neither a student nor a lecturer. With `&&`, if the left operand is `false` the final result is already `false` regardless of the right operand, so Java skips `!isBlocked` (short-circuit evaluation). The right operand is only evaluated when the left operand is `true`.

> [!NOTE]
> Short-circuit evaluation: `||` stops at the first true operand, and `&&` stops at the first false operand.

---

### 2.3 Experiment 3: Nested IF and Logical Operators to Determine Laboratory Access

A student may use the laboratory outside class hours if their status is active and they are not currently under sanction. If this requirement is met, the system performs a second check. Laboratory access is granted if the student has lecturer permission or is a lab assistant.

> [!NOTE]
> This experiment combines two levels of nested selection with the logical operators `&&`, `||`, and `!`.

#### 2.3.1 Program Code Java

```java
package jobsheet6;
import java.util.Scanner;

public class NestedLabAccessAttendanceNo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        boolean isActiveStudent;
        boolean isSanctioned;
        boolean hasLecturerPermit;
        boolean isLabAssistant;

        System.out.print("Is the student active? (true/false): ");
        isActiveStudent = sc.nextBoolean();
        System.out.print("Is the student currently sanctioned? (true/false): ");
        isSanctioned = sc.nextBoolean();
        System.out.print("Does the student have lecturer permission? (true/false): ");
        hasLecturerPermit = sc.nextBoolean();
        System.out.print("Is the student a lab assistant? (true/false): ");
        isLabAssistant = sc.nextBoolean();

        if (isActiveStudent && !isSanctioned) {
            if (hasLecturerPermit || isLabAssistant) {
                System.out.println("Laboratory access granted");
            } else {
                System.out.println("Access denied: lecturer permission or lab assistant status required");
            }
        } else {
            System.out.println("Access denied: student status does not meet the requirement");
        }
        sc.close();
    }
}
```

#### 2.3.2 Execution Result / Screenshot Output

Input combinations used and their outputs (each possible output appears at least once):

| No | isActiveStudent | isSanctioned | hasLecturerPermit | isLabAssistant | Output |
|---|---|---|---|---|---|
| 1 | true | false | true | false | Laboratory access granted |
| 2 | true | false | false | true | Laboratory access granted |
| 3 | true | false | false | false | Access denied: lecturer permission or lab assistant status required |
| 4 | false | false | true | true | Access denied: student status does not meet the requirement |
| 5 | true | true | true | true | Access denied: student status does not meet the requirement |

Example run of combination 3:

```diff
  Is the student active? (true/false): true
  Is the student currently sanctioned? (true/false): false
  Does the student have lecturer permission? (true/false): false
  Is the student a lab assistant? (true/false): false
- Access denied: lecturer permission or lab assistant status required
```

Example run of combination 5:

```diff
  Is the student active? (true/false): true
  Is the student currently sanctioned? (true/false): true
  Does the student have lecturer permission? (true/false): true
  Is the student a lab assistant? (true/false): true
- Access denied: student status does not meet the requirement
```

#### 2.3.3 Answers to Questions / Reflection Questions

$\color{#0969da}{\textbf{Question 1:}}$ **Why is the check `hasLecturerPermit || isLabAssistant` placed inside the first IF?**

$\color{#1a7f37}{\textbf{Answer:}}$ Because it is a second-level requirement that only matters if the student has already passed the first requirement (active and not sanctioned). Lecturer permission or lab assistant status must never be able to override an inactive or sanctioned status. By placing it inside the first IF, an inactive or sanctioned student is rejected immediately (as shown in combinations 4 and 5, where access is denied even though both `hasLecturerPermit` and `isLabAssistant` are true). It also lets the program give a different message for each level.

> [!IMPORTANT]
> Combinations 4 and 5 show that lecturer permission and lab assistant status cannot override an inactive or sanctioned status.

$\color{#0969da}{\textbf{Question 2:}}$ **Explain the function of the `&&`, `||`, and `!` operators in this program.**

$\color{#1a7f37}{\textbf{Answer:}}$

- `&&` in `isActiveStudent && !isSanctioned`: both conditions must be true, so the student must be active and not sanctioned at the same time.
- `!` in `!isSanctioned`: reverses the value, so the condition is true when the student is not sanctioned.
- `||` in `hasLecturerPermit || isLabAssistant`: one of the two is enough, so the student only needs lecturer permission or lab assistant status.

$\color{#0969da}{\textbf{Question 3:}}$ **Can the access requirement be written as a single condition: `isActiveStudent && !isSanctioned && (hasLecturerPermit || isLabAssistant)`? Explain whether the final access decision stays the same.**

$\color{#1a7f37}{\textbf{Answer:}}$ Yes. The access is granted only when the student is active, is not sanctioned, and has lecturer permission or is a lab assistant. This is logically the same as the nested version: in the nested form, access is granted only when the outer condition is true and then the inner condition is true as well, which is exactly an AND of both. So the final decision (granted or denied) stays the same for all input combinations. The difference is only that with a single condition there is just one `else`, so the program cannot tell the reason for the denial.

> [!TIP]
> Both forms are logically equivalent: `A && !B && (C || D)`. Nested IF is chosen when each failure needs its own message.

$\color{#0969da}{\textbf{Question 4:}}$ **What is the advantage of using Nested IF in this case, compared to a single IF, if the system needs to show different reasons for denial?**

$\color{#1a7f37}{\textbf{Answer:}}$ Nested IF separates the requirements into levels, so each level has its own `else` and its own message. The program can tell the student whether access was denied because of the student status (inactive or sanctioned) or because lecturer permission or lab assistant status is missing. A single IF only has one `else`, so it can only print one general denial message. Nested IF also makes the order of checking clearer and the code easier to maintain.

$\color{#0969da}{\textbf{Question 5:}}$ **Create one input combination that causes access to be denied at the first level, and one that causes it to be denied at the second level.**

$\color{#1a7f37}{\textbf{Answer:}}$

- Denied at the first level: `isActiveStudent = true`, `isSanctioned = true`, `hasLecturerPermit = true`, `isLabAssistant = true`. The student is sanctioned, so `isActiveStudent && !isSanctioned` is false. Output: `Access denied: student status does not meet the requirement`.
- Denied at the second level: `isActiveStudent = true`, `isSanctioned = false`, `hasLecturerPermit = false`, `isLabAssistant = false`. The first level passes, but `hasLecturerPermit || isLabAssistant` is false. Output: `Access denied: lecturer permission or lab assistant status required`.

---

## 3. ASSIGNMENT

The following tasks are performed in this Jobsheet:

1. Implement the bookstore discount system flowchart as a Java program using Nested IF and logical operators.
2. Implement the lab-assistant candidate selection system using nested selection and logical operators.

> [!NOTE]
> Both tasks use Nested IF and logical operators, and every failure shows its reason.

### 3.1 Assignment 1: Bookstore Discount System

The discount is decided in two levels. The first level checks the total purchase. The second level checks the customer's membership status, and the discount percentage is decided from both.

| Total purchase | Member | Non-member |
|---|---|---|
| Rp 200.000 or more | 20% | 10% |
| Rp 100.000 - Rp 199.999 | 10% | 5% |
| Below Rp 100.000 | 5% | 0% |

#### 3.1.1 Program Code Java

```java
package jobsheet6;
import java.util.Scanner;

public class Task1BookstoreDiscountAttendanceNo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("--- Bookstore Discount System ---");
        System.out.print("Is the customer a member? (true/false): ");
        boolean isMember = sc.nextBoolean();
        System.out.print("Enter the total purchase (Rp): ");
        int totalPurchase = sc.nextInt();

        double discountPercent;
        String note;

        if (totalPurchase >= 100000) {
            if (isMember && totalPurchase >= 200000) {
                discountPercent = 20;
                note = "Member with purchase of Rp 200.000 or more";
            } else if (isMember || totalPurchase >= 200000) {
                discountPercent = 10;
                note = isMember ? "Member with purchase of Rp 100.000 - Rp 199.999"
                                : "Non-member with purchase of Rp 200.000 or more";
            } else {
                discountPercent = 5;
                note = "Non-member with purchase of Rp 100.000 - Rp 199.999";
            }
        } else {
            if (isMember) {
                discountPercent = 5;
                note = "Member with purchase below Rp 100.000";
            } else {
                discountPercent = 0;
                note = "No discount: non-member with purchase below Rp 100.000";
            }
        }

        double discountAmount = totalPurchase * discountPercent / 100;
        double finalPrice = totalPurchase - discountAmount;

        System.out.println("Discount category : " + note);
        System.out.println("Discount          : " + (int) discountPercent + "%");
        System.out.println("Discount amount   : Rp " + (long) discountAmount);
        System.out.println("Total to pay      : Rp " + (long) finalPrice);
        sc.close();
    }
}
```

#### 3.1.2 Execution Result / Screenshot Output

Member, purchase Rp 250.000:

```
--- Bookstore Discount System ---
Is the customer a member? (true/false): true
Enter the total purchase (Rp): 250000
Discount category : Member with purchase of Rp 200.000 or more
Discount          : 20%
Discount amount   : Rp 50000
Total to pay      : Rp 200000
```

Non-member, purchase Rp 120.000:

```
--- Bookstore Discount System ---
Is the customer a member? (true/false): false
Enter the total purchase (Rp): 120000
Discount category : Non-member with purchase of Rp 100.000 - Rp 199.999
Discount          : 5%
Discount amount   : Rp 6000
Total to pay      : Rp 114000
```

Non-member, purchase Rp 50.000:

```
--- Bookstore Discount System ---
Is the customer a member? (true/false): false
Enter the total purchase (Rp): 50000
Discount category : No discount: non-member with purchase below Rp 100.000
Discount          : 0%
Discount amount   : Rp 0
Total to pay      : Rp 50000
```

Summary of all test cases:

| Member | Total purchase | Discount | Total to pay |
|---|---|---|---|
| true | Rp 250.000 | 20% | Rp 200.000 |
| true | Rp 150.000 | 10% | Rp 135.000 |
| true | Rp 50.000 | 5% | Rp 47.500 |
| false | Rp 250.000 | 10% | Rp 225.000 |
| false | Rp 120.000 | 5% | Rp 114.000 |
| false | Rp 50.000 | 0% | Rp 50.000 |

---

### 3.2 Assignment 2: Lab-Assistant Candidate Selection

#### 3.2.1 Program Code Java

```java
package jobsheet6;
import java.util.Scanner;

public class Task2AssistantSelectionAttendanceNo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("--- Lab Assistant Candidate Selection ---");
        System.out.print("Is the student active? (true/false): ");
        boolean isActiveStudent = sc.nextBoolean();
        System.out.print("Is the student currently under academic sanction? (true/false): ");
        boolean isSanctioned = sc.nextBoolean();

        if (isActiveStudent && !isSanctioned) {
            System.out.print("Enter the Basic Programming grade: ");
            int grade = sc.nextInt();
            System.out.print("Does the student have a programming competency certificate? (true/false): ");
            boolean hasCertificate = sc.nextBoolean();

            if (grade >= 80 || hasCertificate) {
                System.out.println("Requirements met. The student is called for an interview.");
                System.out.print("Enter the interview score: ");
                int interviewScore = sc.nextInt();

                if (interviewScore >= 75) {
                    System.out.println("Congratulations! The student is accepted as a lab assistant");
                } else {
                    System.out.println("Not accepted: the interview score is below 75");
                }
            } else {
                System.out.println("Not accepted: Basic Programming grade is below 80 and there is no programming competency certificate");
            }
        } else if (!isActiveStudent) {
            System.out.println("Not accepted: the student status is not active");
        } else {
            System.out.println("Not accepted: the student is currently under academic sanction");
        }
        sc.close();
    }
}
```

The program has three levels of selection:

1. **Level 1:** `isActiveStudent && !isSanctioned`. If it fails, the inner `else if` / `else` shows whether the reason is an inactive status or an academic sanction.
2. **Level 2:** `grade >= 80 || hasCertificate`. The student needs a grade of at least 80 or a competency certificate. If it fails, the reason is shown.
3. **Level 3:** `interviewScore >= 75`. The student is accepted only if the interview score is at least 75.

> [!IMPORTANT]
> The interview score is asked only after the student passes the first two levels, because only those students are called for an interview.

#### 3.2.2 Execution Result / Screenshot Output

Accepted (grade 85, interview 80):

```diff
  --- Lab Assistant Candidate Selection ---
  Is the student active? (true/false): true
  Is the student currently under academic sanction? (true/false): false
  Enter the Basic Programming grade: 85
  Does the student have a programming competency certificate? (true/false): false
+ Requirements met. The student is called for an interview.
  Enter the interview score: 80
+ Congratulations! The student is accepted as a lab assistant
```

Failed at the interview (grade 70 but has a certificate, interview 70):

```diff
  --- Lab Assistant Candidate Selection ---
  Is the student active? (true/false): true
  Is the student currently under academic sanction? (true/false): false
  Enter the Basic Programming grade: 70
  Does the student have a programming competency certificate? (true/false): true
+ Requirements met. The student is called for an interview.
  Enter the interview score: 70
- Not accepted: the interview score is below 75
```

Failed at the second requirement (grade 70, no certificate):

```diff
  --- Lab Assistant Candidate Selection ---
  Is the student active? (true/false): true
  Is the student currently under academic sanction? (true/false): false
  Enter the Basic Programming grade: 70
  Does the student have a programming competency certificate? (true/false): false
- Not accepted: Basic Programming grade is below 80 and there is no programming competency certificate
```

Failed because the student is not active:

```diff
  --- Lab Assistant Candidate Selection ---
  Is the student active? (true/false): false
  Is the student currently under academic sanction? (true/false): false
- Not accepted: the student status is not active
```

Failed because of an academic sanction:

```diff
  --- Lab Assistant Candidate Selection ---
  Is the student active? (true/false): true
  Is the student currently under academic sanction? (true/false): true
- Not accepted: the student is currently under academic sanction
```

---

## 4. CONCLUSION

The practical work demonstrates how nested selection statements and logical operators are used to build decisions that have several levels in Java programs.

Nested IF is suitable when a requirement must be checked in stages, because the inner condition is only evaluated when the outer condition is true. It also allows the program to show a different reason for every failure. The logical operators help to combine several conditions: `&&` requires all conditions to be true, `||` requires at least one condition to be true, and `!` reverses a condition. Short-circuit evaluation in `&&` and `||` makes Java skip the right operand when the result is already known from the left operand.

> [!IMPORTANT]
> Key takeaway: use Nested IF when requirements must be checked in stages, and use `&&`, `||`, and `!` to combine conditions inside each stage.
