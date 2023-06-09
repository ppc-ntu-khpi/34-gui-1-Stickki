# UI Lab 3

<img src="https://github.com/ppc-ntu-khpi/34-gui-1-Stickki/blob/74a164bc78d21f4f4e35cf83287efaab34d47e95/GUI3/src/tui/1.jpg"/>

```` java
package tui;

import com.mybank.data.DataSource;
import com.mybank.domain.Bank;
import com.mybank.domain.CheckingAccount;
import com.mybank.domain.Customer;

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.GridLayout;
import java.io.IOException;
import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JEditorPane;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class SWINGDemo {

    private final JEditorPane log;
    private final JEditorPane reportText;
    private final JButton show;
    private final JButton report;
    private final JComboBox<String> clients;
    private static final DataSource dataSource = new DataSource("C:\\Users\\palko\\OneDrive\\Рабочий стол\\GUI3\\src\\tui\\test.dat");

    public SWINGDemo() {
        log = new JEditorPane("text/html", "");
        reportText = new JEditorPane("text/html", "");
        log.setPreferredSize(new Dimension(250, 700));
        show = new JButton("Show");
        report = new JButton("Report");
        clients = new JComboBox<String>();
        for (int i = 0; i < Bank.getNumberOfCustomers(); i++) {
            clients.addItem(Bank.getCustomer(i).getLastName() + ", " + Bank.getCustomer(i).getFirstName());
        }
    }

    private void launchFrame() {
        JFrame frame = new JFrame("MyBank clients");
        frame.setLayout(new BorderLayout());
        JPanel cpane = new JPanel();
        cpane.setLayout(new GridLayout(1, 2));

        cpane.add(clients);
        cpane.add(show);
        cpane.add(report);
        frame.add(cpane, BorderLayout.NORTH);
        frame.add(log, BorderLayout.CENTER);
        frame.add(reportText, BorderLayout.AFTER_LAST_LINE);

        show.addActionListener(e -> {
            Customer current = Bank.getCustomer(clients.getSelectedIndex());

            String customerInfo = "&nbsp;<b><span style=\\\"font-size:2em;\\\">" + current.getLastName() + ", " + current.getFirstName() + "</span><hr>";

            for (int i = 0; i < current.getNumberOfAccounts(); i++) {
                String accType = current.getAccount(i) instanceof CheckingAccount ? "Checking" : "Savings";
                customerInfo += "&nbsp;<b>Acc Type: </b>" + accType + "<br>&nbsp;<b>Balance: <span style=\"color:red;\">$" + current.getAccount(i).getBalance() + "</span></b><br>";
            }

            log.setText(customerInfo);
        });

        report.addActionListener(e -> {
            String report = "&nbsp;CUSTOMERS REPORT<hr>";

            for(int i = 0; i < Bank.getNumberOfCustomers(); i++) {
                Customer customer = Bank.getCustomer(i);
                report += "<b>&nbsp;<span style=\\\"font-size:2em;\\\">" + customer.getLastName() + ", " + customer.getFirstName() + "</span><br>";
                for(int j = 0; j < customer.getNumberOfAccounts(); j++) {
                    String accType = customer.getAccount(j) instanceof CheckingAccount ? "Checking" : "Savings";
                    report += "&nbsp;<b>Acc Type: </b>" + accType + "<br>&nbsp;<b>Balance: <span style=\"color:red;\">$"
                            + customer.getAccount(j).getBalance() + "<br></span></b><br>";
                }
                report += "<hr>";
            }

            reportText.setText(report);
        });

        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setResizable(false);
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        // Load data
        try {
            dataSource.loadData();
        } catch (IOException ex) {
            System.out.println("Exception while reading [test.dat] file.");
        }

        SWINGDemo demo = new SWINGDemo();
        demo.launchFrame();
    }

}
````
