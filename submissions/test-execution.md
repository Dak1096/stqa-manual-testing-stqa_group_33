
| Thông tin | |
|---|---|
| **Group** | Group 33 |
| **Date** | 15/6/2026 |
| **Browser** | Chrome 127.0.6533.90 |
| **OS** | Window 10 |

---

## Detailed results:
### REQ-01: Login
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|------------------|---------------------------|-----------------|---------|-----------|----| 
| TC-01 | Login | 1. Redirect to homepage successfully.<br><br>2. App bar correctly displays user name and role. | 1. Redirect to homepage successfully.<br><br>2. App bar correctly displays user name and role: Nguyễn Thủ Thư (Librarian). | Passed | ![TC-01](./images/REQ-01/TC-01.png)| |
| TC-02 | Login | System displays error message when both fields are blank | System displays error message: "Please enter email and Passedword" | Passed | ![TC-02](./images/REQ-01/TC-02.png) | |
| TC-03 | Login | System blocks login and displays an error message when Passedword is blank | System displays error message: "Please enter email and Passedword" | Passed | ![TC-03](./images/REQ-01/TC-03.png) | |
| TC-04 | Login | System displays error message: "Incorrect Passedword" when Passedword is incorrect | System displays error message: "Incorrect Passedword" | Passed | ![TC-04](./images/REQ-01/TC-04.png) | |
| TC-05 | Login | Email case-insensitivity: normalization to lowercase and successful login | System displays error message: "Not find member" | Failed | ![TC-05](./images/REQ-01/TC-05.png) | |
| TC-06 | Login | Passedword input is masked (dots or asterisks) | Passedword is hidden by dots | Passed | ![TC-06](./images/REQ-01/TC-06.png) | |
| TC-07 | Login | Invalid email format: client-side validation prevents submission | System displays "Can not find member" | Failed | ![TC-07](./images/REQ-01/TC-07.png) | BUG-01 |
| TC-08 | Login | Invalid email format: client-side validation shows "Email is invalid" | System displays "Can not find member" | Failed | ![TC-08](./images/REQ-01/TC-08.png) | BUG-01 |
| TC-09 | Login | Invalid email format: client-side validation shows "Email is invalid" | System displays "Can not find member" | Failed | ![TC-09](./images/REQ-01/TC-09.png) | BUG-01 |

### REQ-02: View book list
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|------------------|---------------------------|---------------|-----------|----------|-----| 
| TC-10 | View book list | The system displays the list of all books in the library with complete information: title, author, category, publication year, book code, and status. | List of all book with full information | Passed | ![TC-10](./images/REQ-02/TC-10.png) |  |
| TC-11 | View book list | Member can view the list of all books with complete information: title, author, category, year, code, status. | List of all book with full information | Passed | ![TC-11](./images/REQ-02/TC-11.png) |  |
| TC-12 | View book list | A book with status "Available" is displayed correctly (e.g., BOOK001 shows "Available"). | Book code "BOOK001" is "available" | Passed | ![TC-12](./images/REQ-02/TC-12.png) |  |
| TC-13 | View book list | A book with status "Borrowed" is displayed correctly (e.g., BOOK003 shows "Borrowed"). | "BOOK03" show "Borrowed" | Passed | ![TC-13](./images/REQ-02/TC-13.png) |  |
| TC-14 | View book list | After borrowing a book, its status updates immediately from "Available" to "Borrowed". | "BOOK001" show "borrowed" | Passed | ![TC-14](./images/REQ-02/TC-14_1.png) ![TC-14](./images/REQ-02/TC-14_2.png) |  |
| TC-15 | View book list | After returning a book, its status updates immediately from "Borrowed" to "Available". | "BOOK003" show "available" | Passed | ![TC-15](./images/REQ-02/TC-15.png) |  |
| TC-16 | View book list | Publication year is shown in a valid 4-digit format and is not empty for every book. | No year display invalid | Passed | ![TC-16](./images/REQ-02/TC-16.png) |  |
| TC-17 | View book list | Verify that a book with Lost status is displayed correctly | "BOOK007" show "Lost" | Passed | ![TC-17](./images/REQ-02/TC-17.png) |  |

### REQ-03: Search and filter books
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|------------------|---------------------------|---------------|-----------|----------|-----| 
| TC-18 | Search and filter books | Search by valid book title (e.g., "Flutter") returns books whose titles contain the keyword. | display all book have 'Flutter' in title | Passed | ![TC-18](./images/REQ-03/TC-18.png) |  |
| TC-19 | Search and filter books | Search using lowercase keyword ("flutter") still returns matching books (case-insensitive). | display all book have 'flutter' in title | Passed | ![TC-19](./images/REQ-03/TC-19.png) |  |
| TC-20 | Search and filter books | Search using uppercase keyword ("FLUTTER") returns matching books (case-insensitive). | display all book have 'FLUTTER' in title | Passed | ![TC-20](./images/REQ-03/TC-20.png) |  |
| TC-21 | Search and filter books | Search by author name (e.g., "Nguyễn Minh Đức") returns books by that author. | display all book that have author is "Nguyễn Minh Đức" | Passed | ![TC-21](./images/REQ-03/TC-21.png) |  |
| TC-22 | Search and filter books | Searching with a non-existing keyword ('bljhyea') shows "No books found". | Display "No books found" in the center screen | Passed | ![TC-22](./images/REQ-03/TC-22.png) |  |
| TC-23 | Search and filter books | Display book with 'python'in the name
 | Display book with 'python'in the name
 | Passed | ![TC-23](./images/REQ-03/TC-23.png) |  |
| TC-24 | Search and filter books | Notification "No book found" in the center screen | Book with 'Flutter' in title pops up | Failed | ![TC-24](./images/REQ-03/TC-24.png) | BUG-02 |
| TC-25 | Search and filter books | Display all books with title has "F" | Display all books with title has "A" | Passed | ![TC-25](./images/REQ-03/TC-25.png) |  |
| TC-26 | Search and filter books | Display all books that have category "Công nghệ" | Display all books that have category "Công nghệ" | Passed | ![TC-26](./images/REQ-03/TC-26.png) | |
| TC-27 | Search and filter books | Display books with 'python'in the name and belongs to "Công nghệ" category
 | Display all book with category "Công nghệ" | Failed | ![TC-27](./images/REQ-03/TC-27.png) | BUG-03 |

### REQ-4 Borrow Book
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-28 | Borrow Book | Successfull message and new borrow record | Displays a success message. A borrow record is created with a due date set to exactly +14 days from today | Passed | ![TC-28](./images/REQ-04/TC-28.1.png) ![TC-28](./images/REQ-04/TC-28.2.png)| |
| TC-29 | Borrow Book | Reject the third try to borrow book | Successfull message and a new borrow record | Failed | ![TC-29](./images/REQ-04/TC-29.png) | BUG-04 | 
| TC-30 | Borrow Book | Reject request and display "suspended" message | The system rejects the request and displays "Expired account" message | Failed | ![TC-30](./images/REQ-04/TC-30.png) | BUG-05 |
| TC-31 | Borrow Book | "+" button is hidden and book status is "Borrowed" | The "+" borrow button is completely hidden for this book and status is "Borrowed" | Passed | ![TC-31](./images/REQ-04/TC-31.png) |
| TC-32 | Borrow Book | Display error messge | The system rejects the request and displays the "Expired account" message | Passed | ![TC-32](./images/REQ-04/TC-32.png) |
| TC-33 | Borrow Book | "+" button is hidden and book status is `lost` | The "+" borrow button is completely hidden/disabled and status is "Lost" | Passed | ![TC-33](./images/REQ-04/TC-33.png) |

### REQ-5 Return Book
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-34 | Return Book | success message, status into "Available" | 1.Success message shown. 2.Record status "Returned". 3.BOOK001 status -> "Available". 4.No overdue warning | Passed | ![TC-34](./images/REQ-05/TC-34_1.png) ![TC-34](./images/REQ-05/TC-34_2.png) |
| TC-35 | Return Book | 1. Return is accepted. 2. BR001 status -> "Returned". 3. BOOK003 status -> "Available" | 1. Return is accepted. 2. BR001 status is "Returned". 3. BOOK003 status is "Available" | Passed | ![TC-35](./images/REQ-05/TC-35_1.png) ![TC-35](./images/REQ-05/TC-35_2.png) |
| TC-36 | Return Book | Can not find `"BOOK013"`, no "return" button | 1. Cannot find BOOK013 in MEM002's list. 2. No "Return" button available for BOOK013. |Passed | ![TC-36](./images/REQ-05/TC-36.png) |
| TC-37 | Return Book | 1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" | 1. Before return: BOOK003 = "Borrowed". 2. After return: BOOK003 = "Available" | Passed | ![TC-37](./images/REQ-05/TC-37_1.png) ![TC-37](./images/REQ-05/TC-37_2.png) ![TC-37](./images/REQ-05/TC-37_3.png)|
| TC-38 | Return Book | Success message | displaying message: "Book returned successfully" and follow up with "Sách đã được trả trước đó"| Passed | ![TC-38](./images/REQ-05/TC-38_1.png) ![TC-38](./images/REQ-05/TC-38_2.png)|
| TC-39 | Return Book | Warning on screen | System does not display anything | Failed | ![TC-39](./images/REQ-05/TC-39.png) | BUG-06 |


### REQ-6 Overdue handling
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-40 | Overdue Handling | Success message | Display "Updated: 2 overdue book" | Passed | ![TC-40](./images/REQ-06/TC-40.png) |
| TC-41 | Overdue Handling | Status: "borrowing" -> "overdue" | Book status stays "borrowing" | Failed | ![TC-41](./images/REQ-06/TC-41.png) | |

### REQ-7 Member management
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-42 | Member management | Create new member with success message | Show error message: "Invalid email" | Failed | ![TC-42](./images/REQ-07/TC-42.png) | BUG-07 |
| TC-43 | Member management | No "add member" icon | No "add member" icon | Passed | ![TC-43](./images/REQ-07/TC-43.png) |
| TC-44 | Member management | Error message about fields blank | Display error message "Fullname is not blank" | Failed | ![TC-44](./images/REQ-07/TC-44.png)| BUG-08 |
| TC-45 | Member management | Error message | Display success message | Failed|![TC-45](./images/REQ-07/TC-45.png)| BUG-07 |
| TC-46 | Member management | Error message | Display message: "Email không hợp lệ" | Passed | ![TC-46](./images/REQ-07/TC-46.png) | |
| TC-47 | Member management | Error message like "This email already exists" | Display message: "Invalid email" | Failed | ![TC-47](./images/REQ-07/TC-47.png) | BUG-09|


### REQ-8 - Borrow record lookup
| TC ID | Functional group | Expected result | Actual result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
|TC-48|Borrow record lookup| Member isn't allowed to lookup other member's record|Member can lookup other member's record|Failed|![TC-48](./images/REQ-08/TC-48.png)| BUG-10 | 
|TC-49| Borrow record lookup | List borrow receipts with full information | List borrow receipts with full information | Passed | ![TC-49](./images/REQ-08/TC-49.png) |  |
| TC-50 | Borrow record lookup | List borrow receipts with full information | List borrow receipts with full information | Passed | ![TC-50](./images/REQ-08/TC-50.png) |  |

---


