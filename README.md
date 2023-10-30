import java.util.*;

class Contact {
    private String name;
    private String phoneNumber;

    public Contact(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }

    public String getName() {
        return name;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Phone Number: " + phoneNumber;
    }
}

class ContactManager {
    private Map<String, Contact> contacts;

    public ContactManager() {
        this.contacts = new HashMap<>();
    }

    public void addContact(String name, String phoneNumber) {
        contacts.put(name, new Contact(name, phoneNumber));
    }

    public Contact searchContact(String name) {
        return contacts.get(name);
    }

    public void editContact(String name, String newPhoneNumber) {
        if (contacts.containsKey(name)) {
            Contact contact = contacts.get(name);
            contact.setPhoneNumber(newPhoneNumber);
            contacts.put(name, contact);
            System.out.println("Contact updated successfully.");
        } else {
            System.out.println("Contact not found.");
        }
    }

    public List<Contact> getAllContacts() {
        return new ArrayList<>(contacts.values());
    }

    public List<Contact> searchContacts(String keyword) {
        List<Contact> results = new ArrayList<>();
        for (Contact contact : contacts.values()) {
            if (contact.getName().toLowerCase().contains(keyword.toLowerCase()) || contact.getPhoneNumber().contains(keyword)) {
                results.add(contact);
            }
        }
        return results;
    }

    public void displayAllContacts() {
        List<Contact> sortedContacts = new ArrayList<>(contacts.values());
        sortedContacts.sort(Comparator.comparing(Contact::getName));
        sortedContacts.forEach(System.out::println);
    }
}

public class Main {
    public static void main(String[] args) {
        ContactManager contactManager = new ContactManager();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("Enter 1 to add contact, 2 to search contact, 3 to display all contacts, 4 to edit a contact, or 5 to exit:");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline left-over

            switch (choice) {
                case 1:
                    System.out.println("Enter contact name:");
                    String name = scanner.nextLine();
                    System.out.println("Enter phone number:");
                    String phoneNumber = scanner.nextLine();
                    contactManager.addContact(name, phoneNumber);
                    System.out.println("Contact added successfully.");
                    break;
                case 2:
                    System.out.println("Enter contact name or phone number to search:");
                    String keyword = scanner.nextLine();
                    List<Contact> searchResults = contactManager.searchContacts(keyword);
                    if (searchResults.isEmpty()) {
                        System.out.println("No contacts found.");
                    } else {
                        System.out.println("Search results:");
                        searchResults.forEach(System.out::println);
                    }
                    break;
                case 3:
                    System.out.println("All contacts:");
                    contactManager.displayAllContacts();
                    break;
                case 4:
                    System.out.println("Enter contact name to edit:");
                    String editName = scanner.nextLine();
                    System.out.println("Enter new phone number:");
                    String newPhoneNumber = scanner.nextLine();
                    contactManager.editContact(editName, newPhoneNumber);
                    break;
                case 5:
                    System.out.println("Exiting contact manager.");
                    return;
                default:
                    System.out.println("Invalid choice, please try again.");
            }
        }
    }
}
