## Step 1: Input Domain Modeling — Input Domain Modeling (IDM)

# Test Cases — Test Case Table

| Info | |
|---|---|
| **Group** | Group 33 |
| **Date created** | 27/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |


### IDM - Login (REQ-01)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Does the email present in DB? | Yes | `librarian@library.com` | Login successful |
|  | No | `noone@email.com` | Error: "Account does not exist" |
| Is the password correct? | Correct | `admin123` | Login successful |
|  | Incorrect | `wrongpass` | Error: "Incorrect password" |
| Is the input field empty? | Not empty | (any) | Normal handling |
|  | Empty | `""` | Error: "Please enter email and password" |

---

### IDM - View book list (REQ-02)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Account role? | Librarian | `librarian@library.com` | See all books with full details |
|  | Member | `ba.nguyen@email.com` | See all books with full details |
| Book status? | Available | BOOK001, BOOK002 | Display "Available" |
|  | Borrowed | BOOK003 | Display "Borrowed" |
|  | Lost | BOOK007 | Display "Lost" |
| Complete information? | 5 fields present | BOOK001 | display Title, Author, Category, Year, Status |
| Realtime update? | After borrowing | Borrow BOOK001 | Status immediately changes to "Borrowed" |
|  | After returning | Return BOOK003 | Status immediately changes to "Available" |

---

### IDM - Search and filter books (REQ-03)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Keyword exists? | Yes (Book title) | `"Flutter"` | Display books containing "Flutter" |
|  | Yes (Author) | `"Nguyễn"` | Display books by author "Nguyễn" |
|  | No | `"bljhyea"` | Display "No books found" message |
| Case sensitivity? | Lowercase | `"flutter"` | Results match "Flutter" |
|  | Uppercase | `"FLUTTER"` | Results match "Flutter" |
| Filter by category? | Category has books | Select `"Technology"` | Display only Technology books |
|  | Category has no books | Empty/None | Display "No books found" message |

---

### IDM - Borrow book (REQ-04)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Book status? | Available | BOOK001 | Allow borrowing |
|  | Borrowed | BOOK003 | Deny with error |
|  | Lost | BOOK007 | Deny with error |
| Member status? | Active | MEM002 | Allow borrowing |
|  | Suspended | MEM004 | Deny, display account suspended error |
|  | Expired | MEM005 | Deny, display account expired error |
| Number of currently borrowed books? | < 3 | MEM006 (1 book) | Allow borrowing |
|  | >= 3 | Member has 3 or more borrowed books | Deny, display limit exceeded error |

---

### IDM - Return book (REQ-05)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Does the record belong to the user? | Yes | MEM002 returns BOOK003 | Allow return |
|  | No | MEM002 returns BOOK013 | Hide return option / Deny |
| Time returned? | Within due date | BR003 | Return success |
|  | Overdue | BR001 | Return successful and display overdue warning |
|  | Already returned | BR002 | Hide return option |
| Book status after return? | Successful | BOOK003 | Change to "Available" |
| record status after return? | Successful | BR001 | Change to "Returned" |

---

### IDM - Overdue handling (REQ-06)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Who triggers? | Librarian | `librarian@library.com` | Scan and update overdue records |
|  | Member | Any member | Hide this function |
| Time? | Before due | Today < Due date | Keep status "Borrowed" |
|  | On due date | Today = Due date | Mark as "Overdued" |
|  | After due date | Today > Due date | Mark as "Overdued" |
| Member sees overdue? | Has overdue records | MEM002 | Sees BR001 marked "Overdued" |
|  | No overdue | MEM006 | Sees no overdued records |
| Librarian sees overdue? | After scanning | Librarian | Sees all overdued records |

---

### IDM - Member management (REQ-07) 

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Who can perform? | Librarian | `librarian@library.com` | display form and can perform add |
|  | Member | `ba.nguyen@email.com` | Hide "Members" tab |
| Email is valida? | Valid | `new@email.com` | Accept |
|  | Missing `.` in domain | `new@domain` | Reject, display invalid email |
|  | Missing `@` | `newdomain.com` | Reject, display invalid email |
|  | Empty | `""` | Reject, display required field error |
| Email is unique? | Not existing | `brandnew@email.com` | Add successfully |
|  | Already exists | `ba.nguyen@email.com` | Reject, display email already exists |
| Full name is entered? | Entered | `"Nguyễn Văn A"` | Accept |
|  | Empty | `""` | Reject, display required field error |
| Phone number is entered? | Entered | `"0912345678"` | Accept |
|  | Empty | `""` | Reject, display required field error |

---

### IDM - Borrow record lookup (REQ-08)

| Characteristic | Block | Value | Expected result |
|---|---|---|---|
| Who can look up? | Librarian | `librarian@library.com` | View all records |
|  | Member | `ba.nguyen@email.com` | View only records belonging to MEM002 |
| View another member's records? | Attempt to view others | MEM002 views MEM006 | **Not allowed** |
| record information complete? | Borrowed | BR001 | display ID, Book, Date, "Borrowed" |
|  | Returned | BR002 | display "Returned" |
|  | Overdued | BR001 | display "Overdued" |

## Step 2: Test Cases

### REQ-01 : Login

| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
| TC-01 | Verify successful login with a valid registered account | 1. User is not logged in.<br><br>2. Account exists and is activated in the system. | 1. Access login page.<br><br>2. Enter valid email.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: admin123 | 1. Redirect to homepage successfully.<br><br>2. App bar correctly displays user name and role: Nguyễn Thủ Thư (Librarian). | REQ-01 | EP |
| TC-02 | Verify error message when leaving both email and password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Leave both email and password blank.<br><br>3. Click Login. | Leave both fields blank. | System displays error message: "Please enter email and password". | REQ-01 | Decision table | 
| TC-03 | Verify error message when entering correct email but leaving password blank | User is not logged in. | 1. Access login section of the website.<br><br>2. Enter correct email.<br><br>3. Leave password blank.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: (blank) | System blocks login and displays an error message requiring a password (or displays blank-field notification). | REQ-01 | BVA |
| TC-04 | Verify error message when entering correct email but incorrect password | 1. User is not logged in.<br><br>2. Account exists in the system. | 1. Access login section of the website.<br><br>2. Enter valid email.<br><br>3. Enter incorrect password.<br><br>4. Click Login. | 1. Email: librarian@library.com<br><br>2. Password: hehe | System displays error message: "Incorrect password". | REQ-01 | EP | 
| TC-05 | Verify login behavior when intentionally capitalizing the first letter of the email | 1. User is not logged in.<br><br>2. Account librarian@library.com exists in the system. | 1. Go to login page.<br><br>2. Enter email with capitalized first letter.<br><br>3. Enter correct password.<br><br>4. Click Login. | 1. Email: Librarian@library.com<br><br>2. Password: admin123 | System automatically converts email to lowercase, logs in successfully, and redirects to homepage. | REQ-01 | EP |
| TC-06 | Verify the security/masking of the password input field | User is on the login page. | Type characters into the password field. | Password: admin123 | Entered characters must be masked as dots (●●●●●) or asterisks (*****). | REQ-01 | Blackbox testing |
| TC-07 | Verify email format | User is not logged in. | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarianlibrarycom<br><br>2. Password: any | System displays invalid email format error message and does not send request to server. | REQ-01 | EP |
| TC-08 | Verify email format | User is not logged in | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarian@librarycom<br><br>2. Password: admin123 | System displays error message: "Email is invalid" | REQ-01 | EP |
| TC-09 |  Verify email format | User is not logged in | 1. Access login page.<br><br>2. Enter invalid format email.<br><br>3. Enter any password.<br><br>4. Click Login. | 1. Email: librarianlibrary.com<br><br>2. Password: admin123 | System displays error message: "Email is invalid" | REQ-01 | EP |

### REQ-02 View book list

| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-10 | Verify that the librarian can view the list of all books | 1. Librarian account has logged in successfully.<br><br>2. The data is in the seed data state. | 1. Select the Books tab.<br><br>2. Observe the displayed book list. | Account: librarian@library.com | The system displays the list of all books in the library with complete information: book title, author, category, publication year, and status. | REQ-02 | EP |
| TC-11 | Verify that a member can view the list of all books | 1. Member account has logged in successfully.<br><br>2. The data is in the seed data state. | 1. Select the Books tab.<br><br>2. Observe the displayed book list. | Account: ba.nguyen@email.com | The member can view the book list with complete information: book title, author, category, publication year, and status. | REQ-02 | EP |
| TC-12 | Verify that a book with Available status is displayed correctly | User has logged in successfully. | 1. Select the Books tab.<br><br>2. Find the book BOOK001 - Lập trình Flutter cơ bản.<br><br>3. Check the book status. | Book: BOOK001 | The book displays the status Available. | REQ-02 | EP |
| TC-13 | Verify that a book with Borrowed status is displayed correctly | User has logged in successfully. | 1. Select the Books tab.<br><br>2. Find the book BOOK003 - Kiểm thử phần mềm nhập môn.<br><br>3. Check the book status. | Book: BOOK003 | The book displays the status Borrowed. | REQ-02 | EP |
| TC-14 | Verify that the book status is updated after borrowing | 1. Member account has logged in successfully.<br><br>2. Member account is active.<br><br>3. Book BOOK001 is currently in Available status. | 1. Select the Books tab.<br><br>2. Borrow book BOOK001.<br><br>3. Observe the book status after borrowing. | 1. Account: `ba.nguyen@email.com`<br><br>2. Book: BOOK001 | The book status is updated from Available to Borrowed immediately after borrowing successfully. | REQ-02 | Decision table |
| TC-15 | Verify that the book status is updated after returning | 1. Member account has logged in successfully.<br><br>2. Member is currently borrowing book BOOK001. | 1. Select the Borrow / Return tab.<br><br>2. Return book BOOK001.<br><br>3. Go back to the Books tab.<br><br>4. Check the status of book BOOK001. | 1. Account: `ba.nguyen@email.com`<br><br>2. Book: BOOK001 | The book status is updated from Borrowed to Available after returning successfully. | REQ-02 | Decision table |
| TC-16 | Verify that the publication year is displayed in a valid format | User has logged in successfully. | 1. Select the Books tab.<br><br>2. Observe the publication year information of the books. | None | The publication year of each book is displayed in a valid 4-digit number format and is not empty. | REQ-02 | BVA |
| TC-17 | Verify that a book with Lost status is displayed correctly | User has logged in successfully. | 1. Select the Books tab.<br><br>2. Find the book BOOK007.<br><br>3. Check the book status. | Book: BOOK007 | The book displays the status Lost. | REQ-02 | EP |

### REQ-03 Search and filter books

| TC ID | Test objective | Preconditions   | Executing steps | Input | Expected result | REQ    | Technique |
| ----- | ------ | ------| -------| ---- | ------ | ------ | -------- |
| TC-18 | Search for a book by a valid book title             | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword Flutter.<br><br>3. Observe the search result list.             | Keyword: 'Flutter'                                  | The system displays books whose titles contain Flutter.                                        | REQ-03 | EP             |
| TC-19 | Search for a book title using lowercase letters             | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword flutter.<br><br>3. Observe the search result list.             | Keyword: 'flutter'                                  | The system still displays books containing the keyword Flutter.                                | REQ-03 | EP             |
| TC-20 | Search for a book title using uppercase letters             | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword FLUTTER.<br><br>3. Observe the search result list.             | Keyword: 'FLUTTER'                                  | The system displays the correct search result. Searching is not case-sensitive.                | REQ-03 | EP             |
| TC-21 | Search for books by author                                  | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the author name Nguyễn Minh Đức.<br><br>3. Observe the search result list. | Author: Nguyễn Minh Đức                           | The system displays books written by the corresponding author.                                 | REQ-03 | EP             |
| TC-22 | Search with a non-existing keyword                          | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword 'bljhyea'.<br><br>3. Observe the displayed result.                | Keyword: 'bljhyea'                                   | The system displays the message No books found.                                                | REQ-03 | EP             |
| TC-23 | Combine search and category filter with matching results    | User has logged in successfully and is on the Books tab. | 1. Select the category **Công nghệ**.<br><br>2. Enter the keyword **python**.<br><br>3. Observe the displayed result.      | 1. Category: Công nghệ<br><br>2. Keyword: Python | The system displays books that match both the search keyword and the selected category filter. | REQ-03 | Decision table |
| TC-24 | Combine search and category filter with no matching results | User has logged in successfully and is on the Books tab. | 1. Select the category Kinh tế.<br><br>2. Enter the keyword Flutter.<br><br>3. Observe the displayed result.      | 1. Category: Kinh tế<br><br>2. Keyword: Flutter | The system displays the message No books found.                                                | REQ-03 | Decision table |
| TC-25 | Verify the minimum search keyword length boundary           | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter one character in the search box.<br><br>3. Observe the displayed result.   | Keyword: 'A'                                        | The system still processes the search and displays matching results if available.              | REQ-03 | BVA            |
| TC-26 | Filter books by selecting a valid category | User has logged in successfully and is on the Books tab. | 1. Select the category Công nghệ.<br><br>2. Observe the displayed book list. | Category: Công nghệ | The system displays only books that belong to the Công nghệ category. Books from other categories are not displayed. | REQ-03 | EP |
| TC-27 | Verify search result when entering search keyword first and selecting category after that | User has logged in successfully and is on the Books tab. | 1. Click the search box.<br><br>2. Enter the keyword python.<br><br>3. Select the category Công nghệ.<br><br>4. Observe the displayed result. | 1. Keyword: Python<br><br>2. Category: Công nghệ | The system displays all books that satisfy only the category condition: the book belongs to the Công nghệ category. | REQ-03 | Decision table |

---

### REQ-04 Borrow book

| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
|TC-28|Verify successful book borrowing under limit (<= 3)|Logged in as `biet.hoang@email.com` (currently has 1 active borrow)| 1. Go to "Books" tab.<br><br>2. Click the "+" button for an "Available" book (`BOOK001`) then "Borrow".<br><br>3. Navigate to the "Borrow / Return" tab.<br><br>4. Observe the newly created borrow record |`BOOK001`|Displays a success message. A borrow record is created with a due date set to exactly +14 days from today|REQ-04|EP|
|TC-29|Verify borrow rejection at the boundary limit (= 3)|Logged in as `biet.hoang@email.com` (currently has 2 active borrows)| 1. Borrow `BOOK002` -> Total 3 borows.<br>2. Attempt to borrow `BOOK005`|`"BOOK002"`, `BOOK005`|System allows the first borrow. Rejects the 2nd attempt of `BOOK005` with an error message stating the limit of 3 books is reached|REQ-04|BVA|
|TC-30|Verify borrow rejection for "Suspended" member|Logged in as `cu.le@email.com` (Member status: Suspended)|1. Go to "Books" tab.<br><br>2. Select an "Available" book (`BOOK001`).<br><br>3. Click the"+" button then "Borrow"|`BOOK001`|The system rejects the request and displays a specific error message for the "Suspended" account status|REQ-04|Decision table|
|TC-31|Verify borrow rejection for an already "Borrowed" book|Logged in as `dam.tran@email.com`|1. Go to "Books" tab.<br><br>2. Search or locate `BOOK003` (Status: Borrowed).<br><br>3. Observe the book card|`BOOK003`|The "+" borrow button is completely hidden/disabled for this book and status is "Borrowed"|REQ-04|EP|
|TC-32|Verify borrow rejection for "Expired" member|Logged in as `binh.pham@email.com` (Member status: Expired)|1. Go to "Books" tab.<br><br>2. Select an "Available" book (`BOOK001`).<br><br>3. Click the"+" button then "Borrow"|`BOOK001`|The system rejects the request and displays a specific error message for the "Expired" account status|REQ-04|EP|
|TC-33|Verify borrow rejection for "Lost" book|Member `ba.nguyen@email.com` is logged in|1. Locate `"BOOK007"` (Status: Lost).<br>2. Observe the book card|Book: `"BOOK007"`|The "+" borrow button is completely hidden/disabled and status is "Lost"|REQ-04|EP|


---

### REQ-05 Return book

| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
|TC-34|Return a borrowed book on time, no overdue warning| Logged in `ba.nguyen@email.com`. Just borrowed BOOK001.|1. Go to tab "Borrow/Return".<br><br>2. Find BOOK001 in "My borrow records".<br><br>3. Click "Return book". Confirm if prompted |BOOK001, borrow date = today, due date = today + 14 days|1.Success message displayn. 2.Record status "Returned". 3.BOOK001 status -> "Available". 4.No overdue warning|REQ-05|EP|
|TC-35|Return an overdue book - allowed but displays overdue warning | Logged in librarian@library.com, and Click `"Check overdue books"` <br> Logged in ba.nguyen@email.com. Record BR001 exists: MEM002 borrowed BOOK003, due 15/09/2024|1. Go to "Borrow/Return" tab.<br><br>2. Find "BR001" in "My Borrow records".<br><br>3. Click "Return book" |Record: BR001, BOOK003, due date: 15/09/2024, return date: today  |1. Return is accepted. 2. BR001 status -> "Returned". 3. BOOK003 status -> "Available" |REQ-05|EP |
|TC-36| Cannot return a book borrowed by another member |Logged in `ba.nguyen@email.com`. BOOK013 is borrowed by another member, not MEM002 |1. Go to "Borrow/Return" tab.<br><br>2. Find BOOK013 in "My borrow records" |Account of MEM002 but BOOK013 is borrowed by MEM006 |1. Cannot find BOOK013 in MEM002's list. 2. No "Return" button available for BOOK013. 3. System does not allow returning another member's book |REQ-05 |EP |
|TC-37| Book status updates immediately after return| Logged in as `ba.nguyen@email.com`.  BOOK003 currently displays "Borrowed" in Books tab|1. Go to "Borrow/Return".<br><br>2. Find BR001 in "My borrow records".<br><br>3. Click "Return book".<br><br>4. Immediately go back to "Books" tab.<br><br>5. Find BOOK003 |Record: BOOK003 |1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" — updates immediately |REQ-05 |EP |
| TC-38 | Check the Characteristic to block duplicate requests (spam clicks) when returning books | Log in using your Librarian account | Step 1: Go to the Borrow/Return tab.<br><br>Step 2: Find an active loan slip.<br><br>Step 3: Click the Return button repeatedly and observe the quantity and content of the popup that appears on the screen | Ticket code: `BR003` and the action of clicking repeatedly | The system only accepts and processes the first click request, displaying only one popup message: "Book returned successfully".     | REQ-05 | EP |
| TC-39 | Check if the overdue warning is displayed when the book return is overdue | Log in using `biet.hoang@email.com`   | Step 1: Go to the Borrow/Return tab.<br><br>Step 2: Check if the system displays any overdue warnings  | Loan slip code and book code  | The system is required to display a clear overdue warning on the interface screen  | REQ-05 | EP |

---

### REQ-06 Overdue handling
| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-40 | Check overdue books | A borrow record has `dueDate` ≤ current date<br> (Currently 2 overdue book) | 1. Log in with Librarian account.<br><br>2. Select 'Borrow/Return' tab.<br><br>3. Click 'Check overdue books' button. | Librarian account | 1. Notification 'updated: 2 overdue records' overdue books.<br><br>2. Book statuses change from 'Borrowed' to 'Overdue'. | REQ-06 | STT |
| TC-41 | Member sees their overdue records | A borrow record has `dueDate` ≤ current date | 1. Log in with Member account.<br><br>2. Select 'Borrow/Return' tab. | Member account | Book status for borrowed items changes from 'Borrowed' to 'Overdue' | REQ-06 | STT |

### REQ-07 Member management
| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|----------------|---------------|-----------------|------------|-----------------|-----|-----------|
| TC-42 | Verify successful member addition with Librarian permission | Logged in with Librarian role. | 1. Fill in all valid information.<br><br>2. Click button: Add Member. | 1. Full Name: `Tran Hieu`<br><br>2. Email: `hieu67@gmail.com`<br><br>3. Phone: `0123456789` | New member is created successfully and saved in the system, displaying: "Member added successfully! ID: MEMxxx". | REQ-07 | EP |
| TC-43 | Verify permission block when role is not Librarian | Logged in with a member account. | 1. Log in with a member account.<br><br>2. Observe interface. | 1. Email: `ba.nguyen@email.com`<br><br>2. Password: `password123` | 1. Redirect to login/home page.<br><br>2. "Add Member" button is not visible. | REQ-07 | EP |
| TC-44 | Verify behavior when leaving all mandatory fields blank | Logged in with the Librarian account. | 1. Leave Full Name, Email, and Phone fields blank.<br><br>2. Click Add Member. | All fields blank. | System prevents data submission and displays error prompts for mandatory fields. | REQ-07 | Decision table |
| TC-45 | Add member with invalid email format (has @ but missing dot) | Log in with the Librarian account. | 1. Fill in info with email missing a dot in the domain section.<br><br>2. Click Add Member. | 1. Full Name: `Tran Hieu`<br><br>2. Email: `hieu67@gmailcom`<br><br>3. Phone: `0123456789` | System displays invalid email error message, registration denied. | REQ-07 | EP |
| TC-46 | Add member with invalid email format (has dot but missing @) | Log in with the Librarian account. | 1. Fill in info with email missing @ symbol.<br><br>2. Click Add Member. | 1. Full Name: `Tran Hieu`<br><br>2. Email: `hieu67gmail.com`<br><br>3. Phone: `0123456789` | System displays invalid email error message, registration denied. | REQ-07 | EP | 
| TC-47 | Verify behavior when creating a member with an already existing email | Logged in with the Librarian account. | 1. Use an email already existing on the system (`cu.le@email.com`). | 1. Full Name: `Tran Hieu`<br><br>2. Email: `cu.le@email.com`<br><br>3. Phone: `0123456789` | System blocks and displays "This email already exists". | REQ-07 | EP |


---

### REQ-08 - Borrow record lookup
| TC ID | Test objective | Preconditions | Executing steps | Input | Expected result | REQ | Technique |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
 TC-48 | Member search for another member's record | 'Another member' have borrowed books from the library | 1. Log in with any member account<br><br>2. Select 'Borrow/Return' -> 'Search borrow records' tab. Search for a different member's ID | Member's account | Member should not be able to see another member's borrow records. | REQ-08 | EP |
| TC-49 | Librarian views all borrow records of all members | Members have borrowed books from the library | 1. Log in with Librarian account<br><br>2. Select 'Borrow/Return' tab | Librarian account | The 'All borrow records' tab displays a list of borrow records with book title, record ID, borrower name, borrow date, due date, and the book's borrow status. | REQ-08 | EP |
| TC-50 | Member views all their own borrow records | Member has borrowed books from the library | 1. Log in with a member account<br><br>2. Select 'Borrow/Return' tab | Member account (`ba.nguyen@email.com`) | The 'My borrow records' tab displays all records that belong to the logged-in member, including book title, record ID, member name, borrow date, due date, and book status. | REQ-08 | EP |


## Summary

| Function | Number of TCs | REQs covered | Applied IDM techniques  |
|----------------|------:|-------------|----------------------|
| Login | 9 | REQ-01 | EP, BVA, decision table, blackbox testing |
| View book list | 8 | REQ-02 | EP, decision table, BVA |
| Search and filter books | 10 | REQ-03 | EP, decision table, BVA |
| Borrow book | 6 | REQ-04 | EP, BVA, decision table |
| Return book | 6 | REQ-05 | EP |
| Overdue handling | 2 | REQ-06 | state transition testing (STT) |
| Member management | 6 | REQ-07 | EP, decision table |
| Borrow record lookup | 3 | REQ-08 | EP |
| Total | 50 | REQ-01 -> REQ-08 | EP, BVA, decision table, STT, blackbox testing |