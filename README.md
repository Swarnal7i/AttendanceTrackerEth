# Attendance Tracker

This Solidity program is a simple contract that tracks the attendance of students to demonstrates the basic syntax and functionality of the Solidity programming language. The purpose of this program is to serve as a starting point for those who are new to Solidity and want to get a feel for how it works.

## Description

This program is a simple contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The program has various funtions like MarkAttendance(), UnmarkAttendance(), GetAllAttendace(),etc to implement the logic behind tracking the data.Also require(),Assert(),revert() statements, various events like AttendanceMarked, StudentAdded are used to show hteir functionalities. This program serves as a simple and straightforward introduction to Solidity programming, and can be used as a stepping stone for more complex projects in the future.

## Getting Started

### Executing program

To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., AttendanceTracker.sol). Copy and paste the following code into the file:


``` solidity

// SPDX-License-Identifier: MIT
pragma solidity 0.8.22;

// Creating a simple contract of AttendanceTracker that keeps track when a student is marked present or absent
contract AttendanceTracker {
    address public teacher; // The address of the teacher is the variable storing the address of the contract creator
    mapping(address => bool) private  attendance;
    address[] private  studentList; // List of all students who have had their attendance marked

    // Events
    event AttendanceMarked(address indexed student, bool present);
    event StudentAdded(address indexed student);

    constructor() {
        teacher = msg.sender;
    }

    function markAttendance(address _student) public {
        // Only the teacher can mark attendance
        require(msg.sender == teacher, "Only the teacher can mark attendance");

        // check is student already marked present
        assert(attendance[_student]!=true);

        // Mark the student as present
        attendance[_student] = true;

        // Add the student to the studentList if not already present
        if (!isStudentInList(_student)) {
            studentList.push(_student);
            emit StudentAdded(_student); // Emit event when a student is added
        }


        

        // Emit event when attendance is marked
        emit AttendanceMarked(_student, true);
        
    }

    function unmarkAttendance(address _student) public {
        // Only the teacher canmarkAttendance(an unmark attendance
        require(msg.sender == teacher, "Only the teacher can unmark attendance");

        // Check if the student is already marked as present and if not reverts with the following message 
        if (!attendance[_student]) {
            revert("Student is not marked as present");
        }

        // Unmark the student as present
        attendance[_student] = false;

        // Emit event when attendance is unmarked
        emit AttendanceMarked(_student, false);

        // Assert that the student is now marked as absent
        assert(attendance[_student] == false);
    }

    // Method to check if a student is marked as present or not
    function isMarkedPresent(address _student) public view returns (bool) {
        return attendance[_student];
    }

    // Method to get all attendance records
    function getAllAttendance() public view returns (address[] memory, bool[] memory) {
        bool[] memory attendanceRecords = new bool[](studentList.length);
        for (uint i = 0; i < studentList.length; i++) {
            attendanceRecords[i] = attendance[studentList[i]];
        }
        return (studentList, attendanceRecords);
    }

    // Internal method to check if a student is in the studentList
    function isStudentInList(address _student) internal view returns (bool) {
        for (uint i = 0; i < studentList.length; i++) {
            if (studentList[i] == _student) {
                return true;
            }
        }
        return false;
    }
}

```

To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.18" (or another compatible version), and then click on the "Compile AttendaceTracker.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select the "AttendanceTracker" contract from the dropdown menu, and then click on the "Deploy" button.

Once the contract is deployed, you can interact with it by first pasting the address of the contract as the input in the Markedattendance(), UnmarkedAttendance() function as well as calling the GetAllAttendance() function .  Finally, click on the "transact" button to execute the function and retrieve the output.

## Authors

Swarnali Das Purkayastha  


## License

This project is licensed under the MIT License - see the LICENSE.md file for details
