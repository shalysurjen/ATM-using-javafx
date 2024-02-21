# ATM-using-javafx
import java.util.Random;
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextArea;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;


/**
 * JavaFX App
 */
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance += amount;
    }

    public boolean withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
}

public class App extends Application {

    private BankAccount account;

    @Override
    public void start(Stage primaryStage) {
        account = new BankAccount(1000); // Initial balance

        primaryStage.setTitle("ATM Machine");

        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);
        grid.setHgap(10);
        grid.setVgap(10);
        grid.setPadding(new Insets(25, 25, 25, 25));

        Label balanceLabel = new Label("Balance: $" + account.getBalance());
        grid.add(balanceLabel, 0, 0, 2, 1);

        Label actionLabel = new Label("Select an action:");
        grid.add(actionLabel, 0, 1);

        Button depositButton = new Button("Deposit");
        Button withdrawButton = new Button("Withdraw");

        HBox hbBtn = new HBox(10);
        hbBtn.setAlignment(Pos.CENTER);
        hbBtn.getChildren().addAll(depositButton, withdrawButton);
        grid.add(hbBtn, 0, 2, 2, 1);

        TextField amountField = new TextField();
        amountField.setPromptText("Amount");
        grid.add(amountField, 0, 3);

        Label messageLabel = new Label();
        grid.add(messageLabel, 0, 4, 2, 1);

        depositButton.setOnAction(event -> {
            String amountText = amountField.getText();
            if (!amountText.isEmpty()) {
                double amount = Double.parseDouble(amountText);
                account.deposit(amount);
                updateBalanceLabel(balanceLabel);
                messageLabel.setText("Deposit successful.");
            } else {
                messageLabel.setText("Please enter an amount.");
            }
        });

        withdrawButton.setOnAction(event -> {
            String amountText = amountField.getText();
            if (!amountText.isEmpty()) {
                double amount = Double.parseDouble(amountText);
                if (account.withdraw(amount)) {
                    updateBalanceLabel(balanceLabel);
                    messageLabel.setText("Withdrawal successful.");
                } else {
                    messageLabel.setText("Insufficient funds.");
                }
            } else {
                messageLabel.setText("Please enter an amount.");
            }
        });

        Scene scene = new Scene(grid, 300, 200);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void updateBalanceLabel(Label balanceLabel) {
        balanceLabel.setText("Balance: $" + account.getBalance());
    }

    public static void main(String[] args) {
        launch(args);
    }
}

