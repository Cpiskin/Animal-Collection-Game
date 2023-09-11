import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class AnimalCollectionGame {
    private static JFrame frame;
    private static JLabel scoreLabel;
    private static int score;
    private static String[] animals = {"Duck", "Dog", "Bear", "Elephant", "Giraffe"};

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            // Create the main game window
        	frame = new JFrame("Animal Collection Game");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(800, 600);
            frame.setLayout(null);

            //Create a label to display the score
            scoreLabel = new JLabel("Score: 0");
            scoreLabel.setBounds(10, 10, 100, 30);
            frame.add(scoreLabel);

            //Add a mouse click listener to the game window
            frame.addMouseListener(new MouseAdapter() {
                @Override
                public void mouseClicked(MouseEvent e) {
                    checkForAnimalClick(e.getX(), e.getY());
                }
            });

            //Make the game window visible
            frame.setVisible(true);

            //Start the game loop
            startGameLoop();
        });
    }

    private static void startGameLoop() {
        Timer timer = new Timer(1000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                spawnRandomAnimal();
            }
        });
        timer.start();
    }

    private static void spawnRandomAnimal() {
        int x = new Random().nextInt(frame.getWidth() - 100);
        int y = new Random().nextInt(frame.getHeight() - 100);

        //Choose a random animal
        String randomAnimal = animals[new Random().nextInt(animals.length)];
        
        //Create a label to display the animal
        JLabel animalLabel = new JLabel(randomAnimal);
        animalLabel.setBounds(x, y, 100, 30);
        frame.add(animalLabel);
        frame.revalidate();

        //Create a timer to remove the animal label after 5 seconds
        Timer timer = new Timer(5000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                frame.remove(animalLabel);
                frame.revalidate();
            }
        });
        timer.setRepeats(false);
        timer.start();
    }

    private static void checkForAnimalClick(int x, int y) {
    	// Get all components in the game window
    	Component[] components = frame.getContentPane().getComponents();
       
    	// Iterate through the components to find the clicked animal
    	for (Component component : components) {
            if (component instanceof JLabel && component.getBounds().contains(x, y)) {
                
            	//Remove the clicked animal and update the score
            	
            	frame.remove(component);
                frame.revalidate();
                incrementScore();
            }
        }
    }

    private static void incrementScore() {
        //Increase the score and update the score label
    	score++;
        scoreLabel.setText("Score: " + score);
    }
}
