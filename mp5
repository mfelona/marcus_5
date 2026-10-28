import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Scanner;

public class Main {

    // Book class
    static class Book {
        private int id;
        private String title;
        private int availableCopies;
        private int borrowedCopies;
        private LinkedList<String> waitingPatrons;

        public Book(int id, String title, int availableCopies) {
            this.id = id;
            this.title = title;
            this.availableCopies = availableCopies;
            this.borrowedCopies = 0;
            this.waitingPatrons = new LinkedList<String>();
        }

        public int getId() {
            return id;
        }

        public String getTitle() {
            return title;
        }

        public int getAvailableCopies() {
            return availableCopies;
        }

        public int getBorrowedCopies() {
            return borrowedCopies;
        }

        public LinkedList<String> getWaitingPatrons() {
            return waitingPatrons;
        }

        public boolean borrowCopy() {
            if (availableCopies > 0) {
                availableCopies--;
                borrowedCopies++;
                return true;
            }

            return false;
        }

        public void returnCopy() {
            if (borrowedCopies > 0) {
                borrowedCopies--;
                availableCopies++;
            }
        }

        public void assignReturnedCopy() {
            if (availableCopies > 0) {
                availableCopies--;
                borrowedCopies++;
            }
        }
    }

    private static HashMap<Integer, Book> books =
            new HashMap<Integer, Book>();

    private static HashSet<String> reservationKeys =
            new HashSet<String>();

    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {

        seedBooks();

        int choice = -1;

        while (choice != 0) {

            showMenu();

            choice = readInt("Choose an option: ");

            switch (choice) {

                case 1:
                    borrowBook();
                    break;

                case 2:
                    reserveBook();
                    break;

                case 3:
                    returnBook();
                    break;

                case 4:
                    findBook();
                    break;

                case 5:
                    showWaitingList();
                    break;

                case 0:
                    System.out.println("Program ended.");
                    break;

                default:
                    System.out.println("Invalid menu option.");
            }
        }
    }

    private static void seedBooks() {

        books.put(1, new Book(1, "Java Basics", 1));
        books.put(2, new Book(2, "Algorithms", 0));

        // Third book
        books.put(3, new Book(3, "Data Structures", 1));
    }

    private static void showMenu() {

        System.out.println();
        System.out.println("--- Library Borrowing and Reservation System ---");
        System.out.println("1 Borrow");
        System.out.println("2 Reserve");
        System.out.println("3 Return");
        System.out.println("4 Find");
        System.out.println("5 Show waiting");
        System.out.println("0 Exit");
    }

    private static int readInt(String message) {

        while (true) {

            System.out.print(message);

            String input = scanner.nextLine();

            try {
                return Integer.parseInt(input);
            }
            catch (NumberFormatException e) {
                System.out.println(
                        "Invalid number. Please enter a whole number.");
            }
        }
    }

    private static Book getBook() {

        int id = readInt("Enter book ID: ");

        Book book = books.get(id);

        if (book == null) {
            System.out.println("Book not found.");
        }

        return book;
    }

    private static void borrowBook() {

        Book book = getBook();

        if (book == null) {
            return;
        }

        System.out.print("Enter patron name: ");
        String patron = scanner.nextLine().trim();

        if (patron.isEmpty()) {
            System.out.println("Patron name cannot be empty.");
            return;
        }

        if (book.borrowCopy()) {

            System.out.println(
                    patron + " borrowed " + book.getTitle() + ".");

            System.out.println(
                    "Available: " + book.getAvailableCopies()
                    + ", Borrowed: " + book.getBorrowedCopies());
        }
        else {
            System.out.println("No copies available.");
        }
    }

    private static void reserveBook() {

        Book book = getBook();

        if (book == null) {
            return;
        }

        System.out.print("Enter patron name: ");
        String patron = scanner.nextLine().trim();

        if (patron.isEmpty()) {
            System.out.println("Patron name cannot be empty.");
            return;
        }

        String key = book.getId() + ":" + patron;

        if (reservationKeys.contains(key)) {

            System.out.println("Duplicate reservation rejected.");
            return;
        }

        book.getWaitingPatrons().add(patron);
        reservationKeys.add(key);

        System.out.println(
                patron + " added to waiting list.");

        showWaiting(book);
    }

    private static void returnBook() {

        Book book = getBook();

        if (book == null) {
            return;
        }

        if (book.getBorrowedCopies() == 0) {

            System.out.println("No borrowed copy to return.");
            return;
        }

        book.returnCopy();

        if (!book.getWaitingPatrons().isEmpty()) {

            String patron =
                    book.getWaitingPatrons().removeFirst();

            reservationKeys.remove(
                    book.getId() + ":" + patron);

            book.assignReturnedCopy();

            System.out.println(
                    "Copy assigned to " + patron + ".");
        }
        else {

            System.out.println(
                    "Book returned successfully.");
        }

        System.out.println(
                "Available: " + book.getAvailableCopies()
                + ", Borrowed: " + book.getBorrowedCopies());

        if (!book.getWaitingPatrons().isEmpty()) {
            showWaiting(book);
        }
    }

    private static void findBook() {

        Book book = getBook();

        if (book == null) {
            return;
        }

        System.out.println("ID: " + book.getId());
        System.out.println("Title: " + book.getTitle());
        System.out.println(
                "Available: " + book.getAvailableCopies());
        System.out.println(
                "Borrowed: " + book.getBorrowedCopies());
    }

    private static void showWaitingList() {

        Book book = getBook();

        if (book == null) {
            return;
        }

        showWaiting(book);
    }

    private static void showWaiting(Book book) {

        if (book.getWaitingPatrons().isEmpty()) {

            System.out.println("Waiting list is empty.");
            return;
        }

        System.out.print("Waiting: ");

        boolean first = true;

        for (String patron : book.getWaitingPatrons()) {

            if (!first) {
                System.out.print(", ");
            }

            System.out.print(patron);

            first = false;
        }

        System.out.println();
    }
}
