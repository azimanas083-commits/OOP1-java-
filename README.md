public class Student {
    private String rollNo;    
    private String name;    
    private String grade;
    private double marks;
	private double attendance; 

   public Student(String rollNo, String name, String grade, double marks, double attendance) {
        this.rollNo = rollNo;        
        this.name = name;             
        this.grade = grade;           
        this.marks= marks;
		this.attendance = attendance;
    }
	 
    public void setRollNo(String rollNo) {
        this.rollNo = rollNo;
    }

    
    public void setName(String name) {
        this.name = name;
    }

   
    public void setGrade(String grade) {
        this.grade = grade;
    }

   
    public void setMarks(double marks) {
        this.marks = marks;
    }
	public void setAttendance(double attendance) { 
	this.attendance = attendance;
	}

    
    public String getRollNo() {
        return rollNo;
    }

    
    public String getName() {
        return name;
    }

   
    public String getGrade() {
        return grade;
    }

    public double getMarks() {
        return marks;
    }
	 public double getAttendance() { 
	 return attendance; 
	 }
}



import java.util.ArrayList;
import java.util.List;

public class StudentManager {
    private List<Student> students = new ArrayList<>();

    public boolean addStudent(String rollNo, String name, String grade, double marks, double attendance) {
        for (Student s : students) {
            if (s.getRollNo().equalsIgnoreCase(rollNo)) {
                return false; // Duplicate roll no
            }
        }
        students.add(new Student(rollNo, name, grade, marks, attendance)); // Now matches constructor!
        return true;
    }

    public List<Student> getAllStudents() { return students; }

    public Student searchStudent(String rollNo) {
        for (Student s : students) {
            if (s.getRollNo().equalsIgnoreCase(rollNo)) {
                return s;
            }
        }
        return null;
    }
}








import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.List;


public class StudentManagementGUI extends JFrame implements ActionListener {
    JLabel titleLabel, rollLabel, nameLabel, marksLabel, attendanceLabel, gradeLabel, resultLabel;
    JTextField rollField, nameField, marksField, attendanceField, gradeField, searchField;
    JButton addBtn, viewBtn, searchBtn, clearBtn;
    JTextArea displayArea;
    JPanel inputPanel, actionPanel, displayPanel;

    StudentManager manager = new StudentManager();

    public StudentManagementGUI() {
        super("Student Management System");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        
        titleLabel = new JLabel("Student Management System", JLabel.CENTER);
        titleLabel.setFont(new Font("Serif", Font.BOLD, 24));
        titleLabel.setForeground(Color.BLUE);

        
        inputPanel = new JPanel(null);
        inputPanel.setPreferredSize(new Dimension(500, 200));
        inputPanel.setBackground(Color.black);

        rollLabel = new JLabel("Roll No:");
        rollLabel.setForeground(Color.white);
        rollLabel.setBounds(20, 20, 100, 25);
        inputPanel.add(rollLabel);
        rollField = new JTextField();
        rollField.setBounds(130, 20, 120, 25);
        inputPanel.add(rollField);

        nameLabel = new JLabel("Name:");
        nameLabel.setForeground(Color.white);
        nameLabel.setBounds(270, 20, 100, 25);
        inputPanel.add(nameLabel);
        nameField = new JTextField();
        nameField.setBounds(340, 20, 120, 25);
        inputPanel.add(nameField);

        marksLabel = new JLabel("Marks:");
        marksLabel.setForeground(Color.white);
        marksLabel.setBounds(20, 60, 100, 25);
        inputPanel.add(marksLabel);
        marksField = new JTextField();
        marksField.setBounds(130, 60, 120, 25);
        inputPanel.add(marksField);

        attendanceLabel = new JLabel("Attendance:");
        attendanceLabel.setForeground(Color.white);
        attendanceLabel.setBounds(270, 60, 100, 25);
        inputPanel.add(attendanceLabel);
        attendanceField = new JTextField();
        attendanceField.setBounds(370, 60, 90, 25);
        inputPanel.add(attendanceField);

        gradeLabel = new JLabel("Grade:");
        gradeLabel.setForeground(Color.white);
        gradeLabel.setBounds(20, 100, 100, 25);
        inputPanel.add(gradeLabel);
        gradeField = new JTextField();
        gradeField.setBounds(130, 100, 120, 25);
        gradeField.setEditable(false);
        inputPanel.add(gradeField);

        // Marks Field: Auto-calculate grade when changed
        marksField.addKeyListener(new KeyAdapter() {
            public void keyReleased(KeyEvent e) {
                String marksStr = marksField.getText().trim();
                try {
                    double marks = Double.parseDouble(marksStr);
                    String grade = calculateGrade(marks);
                    gradeField.setText(grade);
                } catch (NumberFormatException ex) {
                    gradeField.setText("");
                }
            }
        });

        // Search bar
        searchField = new JTextField();
        searchField.setToolTipText("Enter roll no to search");
        searchField.setBounds(270, 100, 120, 25);
        inputPanel.add(searchField);

        searchBtn = new JButton("Search");
        searchBtn.setForeground(Color.magenta);
        searchBtn.setBounds(400, 100, 90, 25);
        searchBtn.addActionListener(this);
        inputPanel.add(searchBtn);

        // Action Panel
        actionPanel = new JPanel();
        actionPanel.setPreferredSize(new Dimension(500, 60));
        actionPanel.setBackground(Color.gray);

        addBtn = new JButton("Add Student");
        addBtn.setForeground(Color.green);
        addBtn.addActionListener(this);
        actionPanel.add(addBtn);

        viewBtn = new JButton("View All");
        viewBtn.setForeground(Color.blue);
        viewBtn.addActionListener(this);
        actionPanel.add(viewBtn);

        clearBtn = new JButton("Clear Fields");
        clearBtn.setForeground(Color.red);
        clearBtn.addActionListener(this);
        actionPanel.add(clearBtn);

        // Display Panel
        displayPanel = new JPanel(new BorderLayout());
        displayPanel.setPreferredSize(new Dimension(500, 180));
        displayPanel.setBackground(Color.DARK_GRAY);

        displayArea = new JTextArea();
        displayArea.setFont(new Font("Monospaced", Font.PLAIN, 14));
        displayArea.setEditable(false);
        displayArea.setBackground(Color.black);
        displayArea.setForeground(Color.white);

        JScrollPane scrollPane = new JScrollPane(displayArea);
        displayPanel.add(scrollPane, BorderLayout.CENTER);

        resultLabel = new JLabel(" ");
        resultLabel.setFont(new Font("Comic Sans MS", Font.BOLD, 15));
        resultLabel.setForeground(Color.yellow);
        resultLabel.setHorizontalAlignment(JLabel.CENTER);
        displayPanel.add(resultLabel, BorderLayout.SOUTH);

        // Layout for JFrame
        JPanel centerPanel = new JPanel(new BorderLayout());
        centerPanel.add(inputPanel, BorderLayout.CENTER);
        centerPanel.add(actionPanel, BorderLayout.SOUTH);

        setLayout(new BorderLayout());
        add(titleLabel, BorderLayout.NORTH);
        add(centerPanel, BorderLayout.CENTER);
        add(displayPanel, BorderLayout.SOUTH);

        setSize(650, 600);
        setResizable(false);
        setVisible(true);
    }

    
    private String calculateGrade(double marks) {
        if (marks >= 90) return "A+";
		 else if (marks >= 85) return "A";
        else if (marks >= 80) return "B";
        else if (marks >= 70) return "C";
        else if (marks >= 60) return "D";
        else return "F";
    }

    public void actionPerformed(ActionEvent e) {
        Object src = e.getSource();
        if (src == addBtn) {
            String rollNo = rollField.getText().trim();
            String name = nameField.getText().trim();
            String marksStr = marksField.getText().trim();
            String attendanceStr = attendanceField.getText().trim();
            String grade = gradeField.getText().trim();
            try {
                double marks = Double.parseDouble(marksStr);
                double attendance = Double.parseDouble(attendanceStr);
                if (rollNo.isEmpty() || name.isEmpty() || grade.isEmpty()) {
                    resultLabel.setText("All fields required!");
                    return;
                }
                if (manager.addStudent(rollNo, name, grade, marks, attendance)) {
                    resultLabel.setText("Student added successfully.");
                } else {
                    resultLabel.setText("Roll No already exists.");
                }
            } catch (NumberFormatException ex) {
                resultLabel.setText("Invalid marks or attendance!");
            }
        } 
		else if (src == viewBtn) {
    
    StringBuilder sb = new StringBuilder();
    sb.append("RollNo   Name Grade  Marks Attendance\n");
    sb.append("--------------------------------------\n");
    List<Student> students = manager.getAllStudents();
    if (students.isEmpty()) {
        sb.append("No students available.\n");
    } else {
        for (Student s : students) {
            sb.append(s.getRollNo() + "   "
                    + s.getName() + "   "
                    + s.getGrade() + "   "
                    + s.getMarks() + "   "
                    + s.getAttendance() + "\n");
        }
    }
    displayArea.setText(sb.toString());
    resultLabel.setText("Showing all students.");
} else if (src == searchBtn) {
    // Show one student in a clear list
    String rollNo = searchField.getText().trim();
    Student s = manager.searchStudent(rollNo);
    if (s == null) {
        displayArea.setText("No student found for this Roll No.");
        resultLabel.setText("Search complete.");
    } else {
        String details = "Student Details\n"
            + "Roll No: " + s.getRollNo() + "\n"
            + "Name: " + s.getName() + "\n"
            + "Grade: " + s.getGrade() + "\n"
            + "Marks: " + s.getMarks() + "\n"
            + "Attendance: " + s.getAttendance() + "\n";
        displayArea.setText(details);
        resultLabel.setText("Student found.");
    }
} else if (src == clearBtn) {
    rollField.setText("");
    nameField.setText("");
    marksField.setText("");
    attendanceField.setText("");
    gradeField.setText("");
    searchField.setText("");
    displayArea.setText("");
    resultLabel.setText("Fields cleared.");
        resultLabel.setText("Fields cleared.");
        }
    }

    public static void main(String[] args) {
        new StudentManagementGUI();
    }
} 




