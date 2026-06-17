import java.io.Serializable;

public class Student implements Serializable {

    private int studentId;
    private String name;
    private String department;
    private double gpa;

    public Student(int studentId, String name,
                   String department, double gpa) {
        this.studentId = studentId;
        this.name = name;
        this.department = department;
        this.gpa = gpa;
    }

    public int getStudentId() {
        return studentId;
    }

    public String getName() {
        return name;
    }

    public String getDepartment() {
        return department;
    }

    public double getGpa() {
        return gpa;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    public void setGpa(double gpa) {
        this.gpa = gpa;
    }

    @Override
    public String toString() {
        return studentId + " | " + name + " | " +
                department + " | GPA = " + gpa;
    }
}
