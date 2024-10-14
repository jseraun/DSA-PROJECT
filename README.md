[final1DSA.pdf](https://github.com/user-attachments/files/17367247/final1DSA.pdf)
package phonebooksystem;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

// Class to represent a contact
class Contact {
    String name;
    String phoneNumber;
    String address;
    String gender;
    String email;

    // Constructor to initialize contact details
    Contact(String name, String phoneNumber, String address, String gender, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.address = address;
        this.gender = gender;
        this.email = email;
    }

    // Override toString method to display contact details
    @Override
    public String toString() {
        return String.format("Name: %s, Phone: %s, Address: %s, Gender: %s, Email: %s",
                name, phoneNumber, address, gender, email);
    }
}

// Main class for the phonebook application with GUI
public class Phonebook {
    private ArrayList<Contact> contacts; // List to store contacts
    private JFrame frame;
    private JTextArea displayArea;
    private JTextField nameField, phoneField, addressField, emailField;
    private JComboBox<String> genderComboBox; // Combo box for gender selection

    // Constructor to initialize the phonebook and GUI components
    public Phonebook() {
        contacts = new ArrayList<>();
        createGUI();
    }

    // Method to create the GUI
    private void createGUI() {
        frame = new JFrame("Phonebook");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(600, 400);
        frame.setLayout(new GridBagLayout());
        frame.getContentPane().setBackground(new Color(230, 240, 250));

        GridBagConstraints gbc = new GridBagConstraints();
        gbc.fill = GridBagConstraints.HORIZONTAL;
        gbc.insets = new Insets(5, 5, 5, 5);

        // Title Label
        JLabel titleLabel = new JLabel("Phonebook Application");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 20));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        frame.add(titleLabel, gbc);

        // Input fields for contact details
        gbc.gridwidth = 1;
        gbc.gridy = 1;
        frame.add(new JLabel("Name:"), gbc);
        nameField = new JTextField(15);
        gbc.gridx = 1;
        frame.add(nameField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 2;
        frame.add(new JLabel("Phone:"), gbc);
        phoneField = new JTextField(15);
        gbc.gridx = 1;
        frame.add(phoneField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 3;
        frame.add(new JLabel("Address:"), gbc);
        addressField = new JTextField(15);
        gbc.gridx = 1;
        frame.add(addressField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 4;
        frame.add(new JLabel("Gender:"), gbc);
        // Combo box for gender selection
        String[] genders = {"Male", "Female"};
        genderComboBox = new JComboBox<>(genders);
        gbc.gridx = 1;
        frame.add(genderComboBox, gbc);

        gbc.gridx = 0;
        gbc.gridy = 5;
        frame.add(new JLabel("Email:"), gbc);
        emailField = new JTextField(15);
        gbc.gridx = 1;
        frame.add(emailField, gbc);

        // Text area to display contacts
        displayArea = new JTextArea(10, 40);
        displayArea.setEditable(false);
        displayArea.setBackground(new Color(240, 255, 255));
        JScrollPane scrollPane = new JScrollPane(displayArea);
        gbc.gridx = 0;
        gbc.gridy = 6;
        gbc.gridwidth = 2;
        frame.add(scrollPane, gbc);

        // Panel for buttons
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new FlowLayout());

        JButton insertButton = new JButton("Insert");
        JButton searchButton = new JButton("Search");
        JButton deleteButton = new JButton("Delete");
        JButton updateButton = new JButton("Update");
        JButton displayButton = new JButton("Display All");
        JButton sortButton = new JButton("Sort");

        // Add buttons to the panel
        buttonPanel.add(insertButton);
        buttonPanel.add(searchButton);
        buttonPanel.add(deleteButton);
        buttonPanel.add(updateButton);
        buttonPanel.add(displayButton);
        buttonPanel.add(sortButton);

        // Add button panel to the frame
        gbc.gridy = 7;
        frame.add(buttonPanel, gbc);

        // Button actions
        insertButton.addActionListener(e -> insertContact());
        searchButton.addActionListener(e -> searchContact());
        deleteButton.addActionListener(e -> deleteContact());
        updateButton.addActionListener(e -> updateContact());
        displayButton.addActionListener(e -> displayContacts());
        sortButton.addActionListener(e -> sortContacts());

        frame.setVisible(true);
    }

    // Method to insert a new contact
    private void insertContact() {
        String name = nameField.getText().trim();
        String phone = phoneField.getText().trim();
        String address = addressField.getText().trim();
        String gender = (String) genderComboBox.getSelectedItem(); // Get selected gender
        String email = emailField.getText().trim();

        if (!name.isEmpty() && !phone.isEmpty() && (gender.equals("Male") || gender.equals("Female"))) {
            contacts.add(new Contact(name, phone, address, gender, email));
            displayArea.append("Contact added: " + name + "\n");
            clearFields();
        } else {
            JOptionPane.showMessageDialog(frame, "Name and Phone cannot be empty, and Gender must be Male or Female.");
        }
    }

    // Method to search for a contact by name
    private void searchContact() {
        String name = JOptionPane.showInputDialog("Enter Name to Search:");
        Contact contact = contacts.stream()
                .filter(c -> c.name.equalsIgnoreCase(name))
                .findFirst().orElse(null);

        if (contact != null) {
            displayArea.append("Contact found: " + contact + "\n");
        } else {
            displayArea.append("Contact not found.\n");
        }
    }

    // Method to delete a contact by name
    private void deleteContact() {
        String name = JOptionPane.showInputDialog("Enter Name to Delete:");
        boolean removed = contacts.removeIf(c -> c.name.equalsIgnoreCase(name));

        if (removed) {
            displayArea.append("Contact deleted: " + name + "\n");
        } else {
            displayArea.append("Contact not found.\n");
        }
    }

    // Method to update an existing contact's information
    private void updateContact() {
        String name = JOptionPane.showInputDialog("Enter Name to Update:");
        Contact contact = contacts.stream()
                .filter(c -> c.name.equalsIgnoreCase(name))
                .findFirst().orElse(null);

        if (contact != null) {
            String newPhone = JOptionPane.showInputDialog("Enter New Phone Number:", contact.phoneNumber);
            String newAddress = JOptionPane.showInputDialog("Enter New Address:", contact.address);
            String newGender = (String) JOptionPane.showInputDialog(frame, "Select New Gender:", "Update Gender",
                    JOptionPane.QUESTION_MESSAGE, null, new String[]{"Male", "Female"}, contact.gender);
            String newEmail = JOptionPane.showInputDialog("Enter New Email:", contact.email);

            contact.phoneNumber = newPhone;
            contact.address = newAddress;
            contact.gender = newGender;
            contact.email = newEmail;
            displayArea.append("Contact updated: " + contact + "\n");
        } else {
            displayArea.append("Contact not found.\n");
        }
    }

    // Method to display all contacts
    private void displayContacts() {
        displayArea.setText(""); // Clear display area
        for (Contact contact : contacts) {
            displayArea.append(contact + "\n");
        }
    }

    // Method to sort contacts alphabetically by name
    private void sortContacts() {
        Collections.sort(contacts, Comparator.comparing(c -> c.name));
        displayArea.append("Contacts sorted.\n");
    }

    // Method to clear input fields
    private void clearFields() {
        nameField.setText("");
        phoneField.setText("");
        addressField.setText("");
        genderComboBox.setSelectedIndex(0); // Reset gender selection
        emailField.setText("");
    }

    // Main method to run the application
    public static void main(String[] args) {
        SwingUtilities.invokeLater(Phonebook::new);
    }
}
