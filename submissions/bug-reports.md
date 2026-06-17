| Information | |
|---|---|
| **Group** | Group 33 |
| **Reporting date** | 17/6/2026 |

### Environment
* Browser: Chrome 127.0.6533.90
* Operating System: Windows 06
* Interface Language: English

---

## BUG-01: Incorrect error message and validation logic on the login step

| Attribute       | Details             |
| --------------- | ------------------- |
| **Bug ID** | BUG-01              |
| **Related TC** | TC-07, TC-08, TC-05 |
| **Related REQ** | REQ-01              |
| **Severity** | Medium              |
| **Reported By** | Duong Anh Khoi       |
| **Date Found** | 3/6/2026          |
| **Status** | Open                |

### Steps to Reproduce

#### Case 1 (TC-07)
1. Open the login page.
2. Enter email: `librarianlibrarycom`
3. Enter any password.
4. Click **Login**.

#### Case 2 (TC-08)
1. Open the login page.
2. Enter email: `librarian@librarycom`
3. Enter any password
4. Click **Login**.

#### Case 3 (TC-09)
1. Open the login page.
2. Enter email: `librarianlibrary.com`
3. Enter any password
4. Click **Login**.

### Expected Result
For all invalid email formats:
* The system should validate the email format before sending a request to the server.
* The system should display an error message such as: **"Email không hợp lệ."** 
* Stop the login process.

### Actual Result
For all three scenarios, the system displays:
**"Không tìm thấy thành viên."**

### Impact
* Users may believe that the account does not exist rather than understanding that the email format is incorrect.
* The error message is misleading and does not help users identify the actual input problem.
* The system performs unnecessary account lookup operations before validating user input.

### Evidence
![BUG-01](./images/REQ-01/TC-07.png)
![BUG-01](./images/REQ-01/TC-08.png)
![BUG-01](./images/REQ-01/TC-09.png)


---

## BUG-02: Category is ignored when keyword is entered after searching

| Attribute           | Details                |
| ------------------- | ---------------------- |
| **Bug ID** | BUG-02                |
| **Related TC** | TC-24           |
| **Related REQ** | REQ-03                 |
| **Severity** | Medium                 |
| **Reporter** | Duong Anh Khoi  |
| **Date Discovered** | 3/6/2026             |
| **Status** | Open                   |


### Preconditions
* The user is logged in (any account is fine).
* The user is currently on the `Books` tab.


### Steps to Reproduce
1. Select the category `Kinh tế`.
2. Enter the keyword `Flutter` in the search box.
3. Observe the displayed book list.

### Expected Result
The system should displays: **No books found.**


### Actual Result
* When the user selects `Kinh tế` first then enters `Flutter`, the system completely ignores the category and displays all books (1) with `Flutter` in the title/author.

### Impact
The Search & Filter function gives inconsistent results depending on the input order. Users may receive unrelated books when they select a category before entering a search keyword, making the filter feature unreliable.

### Evidence
![BUG-02](./images/REQ-03/TC-24.png)

---

## BUG-03: Search keyword is ignored when category is entered after searching

| Attribute           | Details                |
| ------------------- | ---------------------- |
| **Bug ID** | BUG-03                 |
| **Related TC** | TC-27           |
| **Related REQ** | REQ-03                 |
| **Severity** | Medium                 |
| **Reporter** | Duong Anh Khoi  |
| **Date Discovered** | 3/6/2026             |
| **Status** | Open                   |


### Preconditions
* The user is logged in (any account is fine).
* The user is currently on the `Books` tab.


### Steps to Reproduce
1. Enter the keyword `python` in the search box.
2. Select the category `Công nghệ`.
3. Observe the displayed book list.

### Expected Result
The displayed books should satisfy both conditions:
* The book belongs to the `Công nghệ` category.
* The book title/author contains the keyword `python`.


### Actual Result
* When the user enters `python` first and then enters `Công nghệ` in category, the system completely ignores the keyword and displays all books under the `Công nghệ` category.

### Impact
The Search & Filter function gives inconsistent results depending on the input order. Users may receive unrelated books when they enter a title before selecting a category, making the filter feature unreliable.

### Evidence
![BUG-03](./images/REQ-03/TC-27.png)

---

## BUG-04: System allows member to borrow 4 books concurrently (Off-by-one boundary limit bypass)

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | BUG-04                                                      |
| **Related TC** | TC-29                                                        |
| **Related REQ** | REQ-08                                                       |
| **Severity** | High  |
| **Reported By** | Duong Anh Khoi                                 |
| **Date Found** | 5/6/2026                                                   |
| **Status** | Open                                                         |

### Preconditions
* Member account `"biet.hoang@email.com"` is logged in.
* The account currently has 2 active borrowed books record in the system after TC-28.

### Steps to Reproduce
1. Go to the "Books" tab.
2. Click the `(+)` button to borrow book `BOOK007` -> total 3 borows.
3. Attempt to borrow a 4th book (`BOOK005`) by clicking its `(+)` button.

### Expected Result
The system must block the 4th book borrow attempt and display a error message stating: "Maximum limit of 3 books reached" to prevent the 4th book from being borrowed.

### Actual Result
The system allows the borrow action for `BOOK005` to process successfully. The member's total active borrowed books increases to 4 without any warning or restriction.

### Impact
Allows users to bypass the business rule constraint. It breaks the library inventory workflow, disrupts book availability tracking, and negatively impacts other members' borrowing privileges.

### Evidence
![Bug 04 Evidence](./images/REQ-04/TC-29.png)

### Proposed Fix
Change the system's restriction of the borrow action into `active_borrows >= 3`.

---

## BUG-05: "Suspended" member account displays incorrect error message stating "Thành viên đã hết hạn" upon borrowing

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | BUG-05                                                       |
| **Related TC** | TC-30                                                        |
| **Related REQ** | REQ-04                                                       |
| **Severity** | High  |
| **Reported By** | Duong Anh Khoi                                 |
| **Date Found** | 5/6/2026                                                   |
| **Status** | Open                                                         |

### Preconditions
* Member account `"cu.le@email.com"` is logged in (Account status is Suspended in Seed Data).

### Steps to Reproduce
1. Go to the "Books" tab.
2. Find any book that is currently marked as **Available**.
3. Click the `(+)` button to attempt to borrow the book.

### Expected Result
The system blocks the action and displays "Suspended member" error message.

### Actual Result
The system blocks the action but displays an incorrect error message: "Thành viên đã hết hạn. Không thể mượn sách." 

### Impact
The system misidentifies the user state workflow. It misleads suspended members about the actual reason their features are locked, making it difficult for administrators or librarians to handle user inquiries accurately.

### Evidence
![Bug 05 Evidence](./images/REQ-04/TC-30.png)

---

## BUG-06: System does not display overdue book return warnings when returns are overdue

| Attribute        | Details                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Bug ID** | BUG-06                                                     |
| **Related TC** | TC-39                                                     |
| **Related REQ** | REQ-05                                                     |
| **Severity** | High                                                       |
| **Reported By** | Duong Anh Khoi                                          |
| **Date Found** | 16/6/2026                                              |
| **Status** | Open                                                      |

### Preconditions
* Logged in as `biet.hoang@email.com` (has 1 overdue book)

### Steps to Reproduce
1. Go to the "Borrowed books" section.
2. Confirm that there are books that are overdue. Click "Return Book" on that overdue book.
3. Confirm returning the book.

### Expected Result
After returning, the system must display a warning such as: "Book is overdue by X days" so user knows.

### Actual Result
The book return process finishes normally; no warnings or penalty notices are displayed on the screen interface.

### Impact
Users are unaware of being fined, leading to surprise and subsequent complaints. Librarians have no explicit prompt basis to process fines immediately because the client interface completely suppresses it, affecting transparency.

### Evidence
![BUG-06](./images/REQ-05/TC-39.png)

## BUG-07: Email validation logic issues

| Attribute       | Details                   |
| --------------- | ------------------------- |
| **Bug ID** | BUG-07                    |
| **Related TC** | TC-42, TC-45                 |
| **Related REQ** | REQ-07                    |
| **Severity** | High                      |
| **Reported By** | Duong Anh Khoi       |
| **Date Found** | 16/6/2026                |
| **Status** | Open                      |


### Preconditions
* Logged in with the Librarian account.
* Open **Add new member**.

### Steps to Reproduce
1. **Case 1**:
   * Enter all valid information.
   * Use a valid email: `hieu67@gmail.com`.
   * Click **Add member**.
2. **Case 2**:
   * Enter all required information.
   * Use an invalid email: `hieu67@gmailcom`.
   * Click **Add member**.

### Expected Result
1. **Case 1**: The member is created successfully.
2. **Case 2**: The system should block the request and display the message: **"Email không hợp lệ."**.

### Actual Result
1. **Case 1**: The system blocks the request and displays **"Email không hợp lệ."**.
2. **Case 2**: The system successfully creates a new member with the invalid email `hieu67@gmailcom`.

### Impact
This violates a core business rule for email validation. Users who enter correct emails cannot register, while invalid emails can be saved into the database, creating invalid records.

### Evidence
![BUG-07](./images/REQ-07/TC-42.png)
![BUG-07](./images/REQ-07/TC-45.png)

---

## BUG-08: The **Add new member** feature only displays the "Full name" error message when the entire form is left blank

| Attribute       | Details       |
| --------------- | ------------- |
| **Bug ID** | BUG-08        |
| **Related TC** | TC-44         |
| **Related REQ** | REQ-07        |
| **Severity** | Low           |
| **Reported By** | Duong Anh Khoi |
| **Date Found** | 16/6/2026    |
| **Status** | Open          |


### Preconditions
* Logged in with the Librarian account.
* Open **Add new member**.

### Steps to Reproduce
1. Don't type anything
2. Click **Add Member**.

### Expected Result
The system should block the request and display validation messages for all empty required fields.

### Actual Result
The system only displays one validation message:
**"Họ tên không được để trống."**.

### Impact
Bad user experience (UX). (Mostly due to user being blind.)

### Evidence
![BUG-08](./images/REQ-07/TC-44.png)

---

## BUG-09: Adding a member with an existing email displays the wrong error message.

| Attribute       | Details                   |
| --------------- | ------------------------- |
| **Bug ID** | BUG-09                    |
| **Related TC** | TC-47              |
| **Related REQ** | REQ-07                    |
| **Severity** | High                      |
| **Reported By** | Duong Anh Khoi             |
| **Date Found** | 17/6/2026                |
| **Status** | Open                      |


### Preconditions
1. Logged in with the Librarian account.
* Open **Add new member**.
### Steps to Reproduce
1. Enter a valid Full Name and Phone Number.
2. Enter the already existing email: `cu.le@email.com`.
3. Click **Add member**.

### Expected Result
The system should detect the duplicate email, prevent account creation, and display the message: **"Email đã tồn tại."**

### Actual Result
The system prevents account creation but displays the message: **"Email không hợp lệ."**

### Impact
This violates the business requirement for error messages. The incorrect message may cause users or librarians to think the email format is wrong, while the actual issue is that the email has already been registered.

### Evidence
![BUG-09](./images/REQ-07/TC-47.png)

---

## BUG-10: Member can see another member's borrow record

| Attribute           | Details                |
| ------------------- | ---------------------- |
| **Bug ID** | BUG-10                 |
| **Related TC** | TC-48           |
| **Related REQ** | REQ-08                |
| **Severity** | High                 |
| **Reporter** | Duong Anh Khoi  |
| **Date Discovered** | 17/6/2026             |
| **Status** | Open                   |


### Preconditions
* The user is logged in on any member account.
* The user is currently on the `Borrow/return` tab.


### Steps to Reproduce
1. Type **MEM007** in the search box
2. Observe the displayed screen

### Expected Result
* The system should not allow members to look for another member's borrow record.


### Actual Result
* The system display the full borrow record of this 'another member'.

### Impact
This is a big security risk. Allowing members to see another member's borrow record is a big data breach, exposing member's private information.

### Evidence
![BUG-10](./images/REQ-08/TC-48.png)

---