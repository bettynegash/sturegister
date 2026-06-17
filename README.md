Student.java
import java.io.Serializable;

public class Student implements Serializable {

    private int id;
    private String name;
    private String department;
    private double gpa;

    public Student(int id, String name, String department, double gpa) {
        this.id = id;
        this.name = name;
        this.department = department;
        this.gpa = gpa;
    }

    public int getId() {
        return id;
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
        return id + " | " + name + " | " + department + " | GPA = " + gpa;
    }
}
Student Management. java
import java.util.ArrayList;

public class StudentManagement {

    private ArrayList<Student> students = new ArrayList<>();

    public void addStudent(Student s) {
        students.add(s);
    }

    public Student searchStudent(int id) {

        for (Student s : students) {

            if (s.getId() == id)
                return s;
        }

        return null;
    }

    public void updateStudent(int id,
                              String name,
                              String department,
                              double gpa) {

        Student s = searchStudent(id);

        if (s != null) {

            s.setName(name);
            s.setDepartment(department);
            s.setGpa(gpa);

            System.out.println("Student updated.");
        }
        else {
            System.out.println("Student not found.");
        }
    }

    public void deleteStudent(int id) {

        Student s = searchStudent(id);

        if (s != null) {

            students.remove(s);
            System.out.println("Student deleted.");
        }
        else {
            System.out.println("Student not found.");
        }
    }

    public void displayAllStudents() {

        for (Student s : students) {
            System.out.println(s);
        }
    }

    public void generateReport() {

        int total = students.size();

        double highest = students.get(0).getGpa();
        double lowest = students.get(0).getGpa();

        double sum = 0;

        for (Student s : students) {

            if (s.getGpa() > highest)
                highest = s.getGpa();

            if (s.getGpa() < lowest)
                lowest = s.getGpa();

            sum += s.getGpa();
        }

        System.out.println("Total Students = " + total);
        System.out.println("Highest GPA = " + highest);
        System.out.println("Lowest GPA = " + lowest);
        System.out.println("Average GPA = " + sum / total);
    }

    public ArrayList<Student> getStudents() {
        return students;
    }
}
file manager.java
import java.io.*;
import java.util.ArrayList;

public class FileManager {

    public static void saveText(ArrayList<Student> students)
            throws Exception {

        PrintWriter pw = new PrintWriter("students.txt");

        for (Student s : students) {
            pw.println(s);
        }

        pw.close();
    }

    public static void saveBinary(ArrayList<Student> students)
            throws Exception {

        DataOutputStream dos =
                new DataOutputStream(
                        new FileOutputStream("students.dat"));

        for (Student s : students) {

            dos.writeInt(s.getId());
            dos.writeDouble(s.getGpa());
        }

        dos.close();
    }

    public static void saveObject(ArrayList<Student> students)
            throws Exception {

        ObjectOutputStream oos =
                new ObjectOutputStream(
                        new FileOutputStream("students.obj"));

        oos.writeObject(students);

        oos.close();
    }

    public static void showFileInfo() {

        File file = new File("students.txt");

        System.out.println("Name : " + file.getName());
        System.out.println("Size : " + file.length() + " bytes");
        System.out.println("Path : " + file.getAbsolutePath());
    }

    public static void createBackup()
            throws Exception {

        BufferedInputStream bis =
                new BufferedInputStream(
                        new FileInputStream("students.obj"));

        BufferedOutputStream bos =
                new BufferedOutputStream(
                        new FileOutputStream("students_backup.obj"));

        int data;

        while ((data = bis.read()) != -1) {

            bos.write(data);
        }

        bis.close();
        bos.close();

        System.out.println("Backup created.");
    }
}
main.java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner input = new Scanner(System.in);

        StudentManagement manager =
                new StudentManagement();

        int choice;

        do {

            System.out.println("\n===== MENU =====");
            System.out.println("1. Add Student");
            System.out.println("2. Search Student");
            System.out.println("3. Update Student");
            System.out.println("4. Delete Student");
            System.out.println("5. Display Students");
            System.out.println("6. Generate Report");
            System.out.println("7. Save Files");
            System.out.println("8. Show File Info");
            System.out.println("9. Create Backup");
            System.out.println("0. Exit");

            System.out.print("Enter choice: ");
            choice = input.nextInt();

            switch (choice) {

                case 1:

                    System.out.print("ID: ");
                    int id = input.nextInt();
                    input.nextLine();

                    System.out.print("Name: ");
                    String name = input.nextLine();

                    System.out.print("Department: ");
                    String dep = input.nextLine();

                    System.out.print("GPA: ");
                    double gpa = input.nextDouble();

                    manager.addStudent(
                            new Student(id, name, dep, gpa));

                    break;

                case 2:

                    System.out.print("Enter ID: ");
                    int searchId = input.nextInt();

                    System.out.println(
                            manager.searchStudent(searchId));

                    break;

                case 3:

                    System.out.print("Enter ID: ");
                    int updateId = input.nextInt();
                    input.nextLine();

                    System.out.print("New Name: ");
                    String newName = input.nextLine();

                    System.out.print("New Department: ");
                    String newDep = input.nextLine();

                    System.out.print("New GPA: ");
                    double newGpa = input.nextDouble();

                    manager.updateStudent(
                            updateId,
                            newName,
                            newDep,
                            newGpa);

                    break;

                case 4:

                    System.out.print("Enter ID: ");
                    int deleteId = input.nextInt();

                    manager.deleteStudent(deleteId);

                    break;

                case 5:

                    manager.displayAllStudents();

                    break;

                case 6:

                    manager.generateReport();

                    break;

                case 7:

                    try {

                        FileManager.saveText(manager.getStudents());
                        FileManager.saveBinary(manager.getStudents());
                        FileManager.saveObject(manager.getStudents());

                        System.out.println("Files saved.");

                    } catch (Exception e) {

                        System.out.println(e.getMessage());
                    }

                    break;

                case 8:

                    FileManager.showFileInfo();

                    break;

                case 9:

                    try {

                        FileManager.createBackup();

                    } catch (Exception e) {

                        System.out.println(e.getMessage());
                    }

                    break;

            }

        } while (choice != 0);

        input.close();
    }
}
