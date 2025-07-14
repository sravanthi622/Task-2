# Task-2
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class GuessNumberGameGUI extends JFrame {
    private int numberToGuess;
    private int attempts;
    private JTextField inputField;
    private JLabel messageLabel;

    public GuessNumberGameGUI() {
        setTitle("🎯 Number Guessing Game");
        setSize(400, 300);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // center on screen
        setLayout(new BorderLayout());

        JLabel heading = new JLabel("Guess the Number (1-100)", SwingConstants.CENTER);
        heading.setFont(new Font("Segoe UI", Font.BOLD, 20));
        heading.setForeground(new Color(46, 134, 222));
        add(heading, BorderLayout.NORTH);

        JPanel centerPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        centerPanel.setBorder(BorderFactory.createEmptyBorder(20, 40, 20, 40));
        centerPanel.setBackground(new Color(245, 249, 255));

        inputField = new JTextField();
        JButton guessButton = new JButton("Submit Guess");
        messageLabel = new JLabel("Enter a number and hit submit!", SwingConstants.CENTER);
        messageLabel.setFont(new Font("Segoe UI", Font.PLAIN, 14));

        guessButton.setBackground(new Color(46, 134, 222));
        guessButton.setForeground(Color.WHITE);
        guessButton.setFont(new Font("Segoe UI", Font.BOLD, 14));
        guessButton.setFocusPainted(false);

        centerPanel.add(inputField);
        centerPanel.add(guessButton);
        centerPanel.add(messageLabel);
        add(centerPanel, BorderLayout.CENTER);

        guessButton.addActionListener(e -> checkGuess());

        resetGame();
        setVisible(true);
    }

    private void resetGame() {
        Random rand = new Random();
        numberToGuess = rand.nextInt(100) + 1;
        attempts = 0;
        inputField.setText("");
        messageLabel.setText("Enter a number and hit submit!");
        messageLabel.setForeground(Color.DARK_GRAY);
    }

    private void checkGuess() {
        String input = inputField.getText();
        try {
            int guess = Integer.parseInt(input);
            attempts++;
            if (guess < 1 || guess > 100) {
                messageLabel.setText("❌ Enter a number between 1 and 100!");
                messageLabel.setForeground(Color.RED);
            } else if (guess == numberToGuess) {
                messageLabel.setText("🎉 Correct! Attempts: " + attempts);
                messageLabel.setForeground(new Color(39, 174, 96));
                int option = JOptionPane.showConfirmDialog(this, "You guessed it! Want to play again?", "Play Again?", JOptionPane.YES_NO_OPTION);
                if (option == JOptionPane.YES_OPTION) {
                    resetGame();
                } else {
                    System.exit(0);
                }
            } else if (guess < numberToGuess) {
                messageLabel.setText("📉 Too low! Try again.");
                messageLabel.setForeground(new Color(230, 126, 34));
            } else {
                messageLabel.setText("📈 Too high! Try again.");
                messageLabel.setForeground(new Color(230, 126, 34));
            }
        } catch (NumberFormatException ex) {
            messageLabel.setText("❗ Please enter a valid number.");
            messageLabel.setForeground(Color.RED);
        }
    }

    public static void main(String[] args) {
        try {
            UIManager.setLookAndFeel("javax.swing.plaf.nimbus.NimbusLookAndFeel");
        } catch (Exception ignored) {}

        new GuessNumberGameGUI();
    }
}
