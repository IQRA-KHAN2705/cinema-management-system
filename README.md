import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.ArrayList;
import java.util.List;

class StaffMember {
    private String name;
    private String work;
    private double monthlySalary;

    public StaffMember(String name, String work, double monthlySalary) {
        this.name = name;
        this.work = work;
        this.monthlySalary = monthlySalary;
    }

    public String getName() {
        return name;
    }

    public String getWork() {
        return work;
    }

    public double getMonthlySalary() {
        return monthlySalary;
    }
}

class Movie {
    private String title;
    private String genre;
    private String duration;
    private String thumbnailPath;
    private int rating;
    private String feedback;
    private String trailerLink;




    public Movie(String title, String genre, String duration, String thumbnailPath, int rating, String feedback, String trailerLink) {
        this.title = title;
        this.genre = genre;
        this.duration = duration;
        this.thumbnailPath = thumbnailPath;
        this.rating = rating;
        this.feedback = feedback;
        this.trailerLink = trailerLink;
    }

    public String getTitle() {
        return title;
    }

    public String getGenre() {
        return genre;
    }

    public String getDuration() {
        return duration;
    }

    public String getThumbnailPath() {
        return thumbnailPath;
    }

    public int getRating() {
        return rating;
    }

    public String getFeedback() {
        return feedback;
    }

    public String getTrailerLink() {
        return trailerLink;
    }

    public void setFeedback(String feedback) {
        this.feedback = feedback;
    }

    @Override
    public String toString() {
        return "Title: " + title + " | Genre: " + genre + " | Duration: " + duration + " | Rating: " + rating;
    }
}

public class CinemaManagementSystem extends JFrame {

    private List<Movie> movies;
    private List<String> foodMenu;
    private List<StaffMember> staffMembers;
    private String movieFeedback;

    public CinemaManagementSystem() {
        setTitle("Cinema Management System");
        setSize(800, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        Font titleFont = new Font("Arial", Font.BOLD, 24);

        // Create a label for the title
        JLabel titleLabel = new JLabel("CINEMA MANAGEMENT SYSTEM", SwingConstants.CENTER);
        titleLabel.setFont(titleFont);
        titleLabel.setFont(new Font("Arial", Font.BOLD, 30));  // font size can be changed..
        titleLabel.setForeground(new Color(255, 215, 0));

        // font white color
        titleLabel.setForeground(Color.WHITE);
        this.movies = createMovies();
        this.foodMenu = createFoodMenu();

        JPanel moviePanel = new JPanel(new GridLayout(0, 1));
        JScrollPane scrollPane = new JScrollPane(moviePanel);
        scrollPane.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);

        JButton adminButton = new JButton("Admin Login");
        JButton customerButton = new JButton("Customer Section");

        adminButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                showAdminLogin();
            }
        });

        customerButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                showCustomerSection();
            }
        });

        JPanel mainPanel = new JPanel(new BorderLayout());
        mainPanel.add(titleLabel, BorderLayout.NORTH);
        mainPanel.add(scrollPane, BorderLayout.CENTER);
        mainPanel.add(createFooterPanel(adminButton, customerButton), BorderLayout.SOUTH);
        mainPanel.setBackground(Color.BLACK);

        add(mainPanel);

        customizeButton(adminButton);
        customizeButton(customerButton);

        populateMoviePanel(moviePanel);

        setVisible(true);
        setLocationRelativeTo(null);
    }

    private void populateMoviePanel(JPanel moviePanel) {
        for (Movie movie : movies) {
            moviePanel.add(createMoviePanel(movie));
        }
    }

    private JPanel createMoviePanel(Movie movie) {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBorder(BorderFactory.createLineBorder(Color.BLACK, 1));

        JLabel thumbnailLabel = new JLabel(new ImageIcon(movie.getThumbnailPath()));
        panel.add(thumbnailLabel, BorderLayout.NORTH);

        JTextArea descriptionArea = new JTextArea(getFormattedDescription(movie));
        descriptionArea.setEditable(false);
        descriptionArea.setLineWrap(true);
        descriptionArea.setWrapStyleWord(true);
        descriptionArea.setForeground(Color.WHITE);  // Set text color to white
        descriptionArea.setBackground(Color.BLACK);
        Font descriptionFont = descriptionArea.getFont();
        descriptionArea.setFont(new Font(descriptionFont.getName(), Font.BOLD, descriptionFont.getSize() + 2));
        panel.add(descriptionArea, BorderLayout.CENTER);

        JLabel ratingLabel = new JLabel("Rating: " + getStarRating(movie.getRating()));
        ratingLabel.setForeground(new Color(255, 215, 0));  // Set text color to golden
        panel.add(ratingLabel, BorderLayout.SOUTH);

        JButton watchTrailerButton = createWatchTrailerButton(movie.getTrailerLink());
        panel.add(watchTrailerButton, BorderLayout.EAST);

        panel.setBackground(Color.BLACK);  // Set background color to black

        return panel;
    }



    private String getFormattedDescription(Movie movie) {
        StringBuilder formattedDescription = new StringBuilder();
        formattedDescription.append("Title: ").append(movie.getTitle()).append("\n");
        formattedDescription.append("Genre: ").append(movie.getGenre()).append("\n");
        formattedDescription.append("Duration: ").append(movie.getDuration()).append("\n");
        formattedDescription.append("Rating: ").append(getStarRating(movie.getRating())).append("\n");
        formattedDescription.append("Description: ").append(movie.getFeedback()).append("\n");
        return formattedDescription.toString();
    }

    private String getStarRating(int rating) {
        StringBuilder stars = new StringBuilder();
        for (int i = 0; i < rating; i++) {
            stars.append("★");
        }
        return stars.toString();
    }

    private JPanel createFooterPanel(JButton adminButton, JButton customerButton) {
        JPanel footerPanel = new JPanel(new GridLayout(1, 2));
        footerPanel.setBackground(Color.DARK_GRAY);

        setButtonForegroundAndFont(adminButton);
        setButtonForegroundAndFont(customerButton);

        footerPanel.add(adminButton);
        footerPanel.add(customerButton);

        return footerPanel;
    }

    private void setButtonForegroundAndFont(JButton button) {
        button.setForeground(Color.WHITE);
        button.setFont(new Font("Arial", Font.BOLD, 16));
    }

    private void customizeButton(JButton button) {
        button.setBackground(new Color(70, 130, 180));
        button.setFocusPainted(false);
    }

    private JButton createWatchTrailerButton(String trailerLink) {
        JButton watchTrailerButton = new JButton("Watch Trailer");
        ImageIcon playIcon = new ImageIcon("path_to_play_icon.png");
        watchTrailerButton.setIcon(playIcon);
        watchTrailerButton.setHorizontalTextPosition(SwingConstants.LEFT);

        watchTrailerButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                watchTrailer(trailerLink);
            }
        });

        customizeButton(watchTrailerButton);
        return watchTrailerButton;
    }

    private void watchTrailer(String trailerLink) {
        try {
            Desktop.getDesktop().browse(new URI(trailerLink));
        } catch (IOException | URISyntaxException e) {
            e.printStackTrace();
        }
    }

    private List<Movie> createMovies() {
        List<Movie> movies = new ArrayList<>();
        movies.add(new Movie("Sholay", "Action", "3 hours", "C:\\Users\\G\\Desktop\\cin_pics\\shol.jpg", 4, "Classic Bollywood action film. The film follows two criminals, Veeru and Jai, hired by a retired police officer to capture the ruthless dacoit Gabbar Singh.", "https://www.youtube.com/watch?v=zzTUvWfvlBg"));
        movies.add(new Movie("Oppenheimer", "Drama", "2.5 hours", "C:\\Users\\G\\Desktop\\cin_pics\\oppen.jpg", 3, "Biographical drama on J. Robert Oppenheimer. The film explores the life and career of the physicist who played a key role in the development of the atomic bomb during World War II.", "https://www.youtube.com/watch?v=uYPbbksJxIg&t=5s"));
        movies.add(new Movie("Barbie", "Animation", "1.5 hours", "C:\\Users\\G\\Desktop\\cin_pics\\b.jpg", 5, "Join Barbie and her friends in this animated adventure. A story of friendship, courage, and fun as Barbie embarks on a magical journey.", "https://www.youtube.com/watch?v=pBk4NYhWNMM"));
        movies.add(new Movie("Fast n Furious", "Action", "2 hours", "C:\\Users\\G\\Desktop\\cin_pics\\fast.jpg", 4, "High-octane action with fast cars and adrenaline-pumping stunts. Join the Fast and Furious crew in their latest mission.", "https://www.youtube.com/watch?v=eoOaKN4qCKw"));
        movies.add(new Movie("Lion King", "Animation", "1.75 hours", "C:\\Users\\G\\Desktop\\cin_pics\\lion.jpg", 5, "A timeless Disney classic featuring Simba, a young lion prince, on a journey of self-discovery and redemption.", "https://www.youtube.com/watch?v=7TavVZMewpY"));
        movies.add(new Movie("Tarzan", "Adventure", "1.8 hours", "C:\\Users\\G\\Desktop\\cin_pics\\tar.jpg", 4, "Join Tarzan, the man raised by gorillas, in this thrilling adventure as he discovers his human heritage and faces challenges in the jungle.", "https://www.youtube.com/watch?v=ie53R2HEZ6g"));

        return movies;
    }

    private List<String> createFoodMenu() {
        List<String> foodMenu = new ArrayList<>();
        foodMenu.add("FROM RIVER TO THE SEE PALESTIN WILL BE FREE");
        foodMenu.add("ISRAELI PRODUCTS NOT AVAILABLE - $0.00");
        foodMenu.add("Local Cola - $5.00");
        foodMenu.add("Salted Popcorn - $5.00");
        foodMenu.add("Caramel Popcorn - $7.00");
        foodMenu.add("Soda - $3.00");
        foodMenu.add("Dairy Milk - $4.00");

        return foodMenu;
    }

    private void showAdminLogin() {
        String username = JOptionPane.showInputDialog("Enter username:");
        String password = JOptionPane.showInputDialog("Enter password:");

        if ("abcd".equals(username) && "abc".equals(password)) {

            staffMembers = createStaffMembers();

            SwingUtilities.invokeLater(new Runnable() {
                @Override
                public void run() {
                    new AdminDashboard(CinemaManagementSystem.this, movies, staffMembers);
                }
            });
        } else {
            JOptionPane.showMessageDialog(null, "Invalid credentials. Please try again.");
        }
    }

    private List<StaffMember> createStaffMembers() {
        List<StaffMember> staffMembers = new ArrayList<>();
        staffMembers.add(new StaffMember("Khosa k", "Janitor", 4500.0));
        staffMembers.add(new StaffMember("Hamza Naeem", "Ticket Sales", 2000.0));
        staffMembers.add(new StaffMember("Alice", "Accounts manager", 3000.0));
        staffMembers.add(new StaffMember("Smith", "Electrician", 2000.0));
        return staffMembers;
    }


    private void showCustomerSection() {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new CustomerSection(CinemaManagementSystem.this, movies, foodMenu);
            }
        });
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new CinemaManagementSystem();
            }
        });
    }
    class AdminDashboard extends JFrame {
        private CinemaManagementSystem parent;
        private List<Movie> movies;
        private List<StaffMember> staffMembers;

        public AdminDashboard(CinemaManagementSystem parent, List<Movie> movies, List<StaffMember> staffMembers) {
            this.parent = parent;
            this.movies = movies;
            this.staffMembers = staffMembers;

            setTitle("Admin Dashboard");
            setSize(400, 300);
            setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);

            JButton viewStatisticsButton = new JButton("View Statistics");
            viewStatisticsButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    viewStatistics();
                }
            });

            JButton viewFeedbackButton = new JButton("View Feedback");
            viewFeedbackButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    viewFeedback();
                }
            });

            JButton viewStaffMembersButton = new JButton("View Staff Members");
            viewStaffMembersButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    viewStaffMembers();
                }
            });

            JPanel mainPanel = new JPanel(new GridLayout(3, 1));
            mainPanel.add(viewStatisticsButton);
            mainPanel.add(viewFeedbackButton);
            mainPanel.add(viewStaffMembersButton);

            add(mainPanel);

            setVisible(true);
            setLocationRelativeTo(null);
        }

        private void viewStatistics() {
            StringBuilder statistics = new StringBuilder("Movie Statistics:\n");

            for (Movie movie : movies) {
                int watchedTimes = getRandomNumber(500, 2000);
                boolean inHotlist = getRandomBoolean();
                boolean trending = getRandomBoolean();
                double revenue = getRandomDouble(3000, 8000);

                statistics.append(movie.getTitle())
                        .append(" - Watched: ").append(watchedTimes).append(" times | ")
                        .append("Hotlist: ").append(inHotlist).append(" | ")
                        .append("Trending: ").append(trending).append(" | ")
                        .append("Revenue: $").append(revenue).append("\n");
            }

            JOptionPane.showMessageDialog(null, statistics.toString());
        }

        private int getRandomNumber(int min, int max) {
            return (int) (Math.random() * (max - min + 1) + min);
        }

        private boolean getRandomBoolean() {
            return Math.random() < 0.5;
        }

        private double getRandomDouble(double min, double max) {
            return Math.random() * (max - min) + min;
        }

        private void viewFeedback() {
            StringBuilder feedbacks = new StringBuilder("Customer Feedbacks:\n");
            for (Movie movie : movies) {
                if (movie.getFeedback() != null && !movie.getFeedback().isEmpty()) {
                    feedbacks.append(movie.getTitle()).append(": ").append(movie.getFeedback()).append("\n");
                }
            }
            JOptionPane.showMessageDialog(null, feedbacks.toString());
        }

        private void viewStaffMembers() {
            StringBuilder staffInfo = new StringBuilder("Staff Members:\n");
            for (StaffMember staffMember : staffMembers) {
                staffInfo.append("Name: ").append(staffMember.getName())
                        .append(" | Work: ").append(staffMember.getWork())
                        .append(" | Monthly Salary: $").append(staffMember.getMonthlySalary())
                        .append("\n");
            }
            JOptionPane.showMessageDialog(null, staffInfo.toString());
        }
    }


    class CustomerSection extends JFrame {
        private CinemaManagementSystem parent;
        private List<Movie> movies;
        private List<String> foodMenu;

        public CustomerSection(CinemaManagementSystem parent, List<Movie> movies, List<String> foodMenu) {
            this.parent = parent;
            this.movies = movies;
            this.foodMenu = foodMenu;

            setTitle("Customer Section");
            setSize(800, 600);
            setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);

            JButton orderFoodButton = new JButton("Order Food");
            orderFoodButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    orderFood();
                }
            });

            JButton bookTicketsButton = new JButton("Book Tickets");
            bookTicketsButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    bookTickets();
                }
            });

            JButton provideFeedbackButton = new JButton("Provide Feedback");
            provideFeedbackButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    provideFeedback();
                }
            });

            JButton viewRecommendationsButton = new JButton("View Recommendations");
            viewRecommendationsButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    viewRecommendations();
                }
            });

            JPanel mainPanel = new JPanel(new GridLayout(2, 2));
            mainPanel.add(orderFoodButton);
            mainPanel.add(bookTicketsButton);
            mainPanel.add(provideFeedbackButton);
            mainPanel.add(viewRecommendationsButton);

            add(mainPanel);

            setVisible(true);
            setLocationRelativeTo(null);
        }

        private void orderFood() {
            StringBuilder foodItems = new StringBuilder("Food Menu:\n");
            for (String food : foodMenu) {
                foodItems.append(food).append("\n");
            }
            String selectedFood = (String) JOptionPane.showInputDialog(null, foodItems.toString(), "Order Food",
                    JOptionPane.QUESTION_MESSAGE, null, foodMenu.toArray(), foodMenu.get(0));
            if (selectedFood != null) {
                JOptionPane.showMessageDialog(null, "Ordered: " + selectedFood);
            }
        }

        private void bookTickets() {
            StringBuilder moviesList = new StringBuilder("Movie List:\n");
            for (Movie movie : movies) {
                moviesList.append(movie.getTitle()).append("\n");
            }

            String selectedMovie = (String) JOptionPane.showInputDialog(
                    null, moviesList.toString(), "Book Tickets",
                    JOptionPane.QUESTION_MESSAGE, null, getMovieTitles(), getMovieTitles()[0]);

            if (selectedMovie != null) {
                int numberOfTickets = getNumberOfTickets();
                int ticketPrice = generateTicketPrice();  // New method to generate random ticket price

                int totalCost = calculateTotalCost(numberOfTickets, ticketPrice);
                int totalCostWithGST = addGST(totalCost);

                generateBill(selectedMovie, numberOfTickets, ticketPrice, totalCostWithGST);
            }
        }

        private int generateTicketPrice() {
            // Generate a random ticket price between 800 and 1500
            return 800 + (int) (Math.random() * 700);
        }

        private int calculateTotalCost(int numberOfTickets, int ticketPrice) {
            return numberOfTickets * ticketPrice;
        }

        private int addGST(int totalCost) {
            // Add 17% GST
            return totalCost + (int) (0.17 * totalCost);
        }

        private void generateBill(String selectedMovie, int numberOfTickets, int ticketPrice, int totalCostWithGST) {
            StringBuilder bill = new StringBuilder("Booking Summary:\n");
            bill.append("Movie: ").append(selectedMovie).append("\n")
                    .append("Number of Tickets: ").append(numberOfTickets).append("\n")
                    .append("Ticket Price: ").append(ticketPrice).append("\n")
                    .append("Total Cost: ").append(totalCostWithGST).append("\n")
                    .append("GST (17%): ").append(totalCostWithGST - calculateTotalCost(numberOfTickets, ticketPrice)).append("\n")
                    .append("Grand Total: ").append(totalCostWithGST);

            JOptionPane.showMessageDialog(null, bill.toString(), "Booking Summary", JOptionPane.INFORMATION_MESSAGE);
        }




        private String[] getMovieTitles() {
            String[] titles = new String[movies.size()];
            for (int i = 0; i < movies.size(); i++) {
                titles[i] = movies.get(i).getTitle();
            }
            return titles;
        }

        private int getNumberOfTickets() {
            String input = JOptionPane.showInputDialog("Enter number of tickets:");
            try {
                return Integer.parseInt(input);
            } catch (NumberFormatException e) {
                return 0;
            }
        }
        private void provideFeedback() {
            StringBuilder moviesList = new StringBuilder("Movie List:\n");
            for (Movie movie : movies) {
                moviesList.append(movie.getTitle()).append("\n");
            }
            String selectedMovie = (String) JOptionPane.showInputDialog(null, moviesList.toString(), "Provide Feedback",
                    JOptionPane.QUESTION_MESSAGE, null, getMovieTitles(), getMovieTitles()[0]);
            if (selectedMovie != null) {
                String feedback = JOptionPane.showInputDialog("Enter feedback for " + selectedMovie + ":");
                if (feedback != null && !feedback.isEmpty()) {
                    updateFeedback(selectedMovie, feedback);
                    JOptionPane.showMessageDialog(null, "Feedback submitted successfully!");
                } else {
                    JOptionPane.showMessageDialog(null, "Feedback cannot be empty. Please try again.");
                }
            }
        }

        private void updateFeedback(String selectedMovie, String feedback) {
            for (Movie movie : movies) {
                if (movie.getTitle().equals(selectedMovie)) {
                    movie.setFeedback(feedback);
                    break;
                }
            }
        }

        private void viewRecommendations() {

            String recommendations = "Recommended Movies:\n";
            recommendations += "1. Sholay\n";
            recommendations += "2. Barbie\n";
            recommendations += "3. Tarzan\n";

            JOptionPane.showMessageDialog(null, recommendations);
        }

        private void viewStaffMembers() {
            StringBuilder staffInfo = new StringBuilder("Staff Members:\n");
            for (StaffMember staffMember : staffMembers) {
                staffInfo.append("Name: ").append(staffMember.getName())
                        .append(", Work: ").append(staffMember.getWork())
                        .append(", Salary: $").append(staffMember.getMonthlySalary())
                        .append("\n");
            }
            JOptionPane.showMessageDialog(null, staffInfo.toString());
        }
    }}
