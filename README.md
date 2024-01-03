Project :=  Retrieve Student Data from a Text File with the Option of Sorting and Searching

Code:-
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace RainbowSchoolsPrototype
{
    class Program
    {
        static void Main()
        {
            string filePath = "students.txt";
            List<Student> students = ReadStudentData(filePath);

            Console.WriteLine("Student List:");

            DisplayStudentList(students);







            bool continueRunning = true;

            while (continueRunning)
            {








                Console.WriteLine("\nOptions:");
                Console.WriteLine("1. Sort by Name");
                Console.WriteLine("2. Search by Name");
                Console.WriteLine("3. Exit");

                Console.Write("Enter your choice (1, 2, or 3): ");
                int choice = int.Parse(Console.ReadLine());

                switch (choice)
                {
                    case 1:
                        students = SortStudentsByName(students);
                        Console.WriteLine("\nSorted Student List by Name:");
                        DisplayStudentList(students);
                        break;

                    case 2:
                        Console.Write("\nEnter the name to search: ");
                        string searchName = Console.ReadLine();
                        SearchStudentByName(students, searchName);
                        break;

                    case 3:
                        Console.WriteLine("Exiting the program. Goodbye!");
                        break;

                    default:
                        Console.WriteLine("Invalid choice. Please enter 1, 2, or 3.");
                        break;
                }



                // Open notepad and wait for it to exit
                OpenNotepadAndWait();










                Console.WriteLine("Do you want to continue? (Yes/No): ");
                char continueChoice = Console.ReadLine().ToUpper()[0];
                continueRunning = (continueChoice == 'Y');






            }

        }

        static List<Student> ReadStudentData(string filePath)
        {
            List<Student> students = new List<Student>();

            try
            {
                string[] lines = File.ReadAllLines("F:/MPHASIS DOCUMENTS/RainbowSchoolsPrototype/RainbowSchoolsPrototype/RainbowSchoolsPrototype/students.txt.txt");

                foreach (var line in lines)
                {
                    string[] data = line.Split(',').Select(s => s.Trim()).ToArray();
                    students.Add(new Student { Name = data[0], Class = data[1] });
                }
            }
            catch (FileNotFoundException)
            {
                Console.WriteLine($"File not found: {filePath}");
            }

            return students;
        }


        static List<Student> SortStudentsByName(List<Student> students)
        {
            return students.OrderBy(s => s.Name).ToList();
        }

        static void DisplayStudentList(List<Student> students)
        {
            foreach (var student in students)
            {
                Console.WriteLine($"{student.Name}, {student.Class}");
            }
        }

        static void SearchStudentByName(List<Student> students, string searchName)
        {
            var result = students.Where(s => s.Name.IndexOf(searchName, StringComparison.OrdinalIgnoreCase) >= 0).ToList();

            if (result.Count() > 0)
            {
                Console.WriteLine($"\nSearch Results for '{searchName}':");
                DisplayStudentList(result);
            }
            else
            {
                Console.WriteLine($"\nNo student found with the name '{searchName}'.");
            }
        }


        static void OpenNotepadAndWait()
        {
            try
            {
                // Specify the path to Notepad.exe
                string notepadPath = Path.Combine(Environment.SystemDirectory, "notepad.exe");

                // Start Notepad process
                Process notepadProcess = Process.Start(notepadPath);

                // Wait for Notepad process to exit
                notepadProcess.WaitForExit();
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error opening Notepad: {ex.Message}");
            }
        }

    }

    class Student
    {
        public string Name { get; set; }
        public string Class { get; set; }
    }


}





