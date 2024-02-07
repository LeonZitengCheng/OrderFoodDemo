//# OrderFoodDemo
//Simple food ordering system
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

// Define an interface for basic order operations
interface OrderOperations {
    void selectProduct();
    void selectDeliveryDateTime();
    void displaySelection();
    void saveToFile() throws IOException;
}

// Abstract class to handle common properties of an order
abstract class Order {
    String customerName;
    String product;
    String deliveryDate;
    String deliveryTime;

    public Order(String customerName) {
        this.customerName = customerName;
    }

    abstract void showOrderSummary();
}

// Implement the interface in a concrete class
class FoodOrder extends Order implements OrderOperations {
    private static final String[] MENU_ITEMS = {"Pizza", "Burger", "Sushi"};
    private static final String[] TIME_SLOTS = {"12:00", "13:00", "18:00", "20:00"};

    public FoodOrder(String customerName) {
        super(customerName);
    }

    // Let user select a product
  public void selectProduct() {
      Scanner scanner = new Scanner(System.in);
      int choice = 0;
      boolean validChoice = false;

      while (!validChoice) {
          System.out.println("Please select a product:");
          for (int i = 0; i < MENU_ITEMS.length; i++) {
              System.out.println((i + 1) + ". " + MENU_ITEMS[i]);
          }

          if (scanner.hasNextInt()) {
              choice = scanner.nextInt();
              // Check if the choice is within the valid range of menu items
              if (choice >= 1 && choice <= MENU_ITEMS.length) {
                  validChoice = true;
              } else {
                  System.out.println("You have entered an invalid number. Please try again.");
              }
          } else {
              // If the user doesn't enter an integer, consume the invalid input and prompt again
              System.out.println("Invalid input. Please enter a number.");
              scanner.next(); // Consume the non-integer input
          }
      }

      // Assign the chosen product
      this.product = MENU_ITEMS[choice - 1];
  }
    // Let user select delivery date and time
  public void selectDeliveryDateTime() {
      Scanner scanner = new Scanner(System.in);
    System.out.println("Please enter the delivery date (MM-DD-YYYY):");
    this.deliveryDate = scanner.nextLine();
      int choice = 0;
      boolean validChoice = false;

      while (!validChoice) {
          System.out.println("Select a delivery time slot:");
          for (int i = 0; i < TIME_SLOTS.length; i++) {
              System.out.println((i + 1) + ". " + TIME_SLOTS[i]);
          }

          if (scanner.hasNextInt()) {
              choice = scanner.nextInt();
              // Check if the choice is within the valid range of time slots
              if (choice >= 1 && choice <= TIME_SLOTS.length) {
                  validChoice = true;
              } else {
                  System.out.println("You have entered an invalid number for the time slot. Please try again.");
              }
          } else {
              // If the user doesn't enter an integer, consume the invalid input and prompt again
              System.out.println("Invalid input. Please enter a number for the time slot.");
              scanner.next(); // Consume the non-integer input
          }
      }

      // Assign the chosen time slot
      this.deliveryTime = TIME_SLOTS[choice - 1];

      // For simplicity, we're just going to use today's date as the delivery date
    
  }


    // Display user's current selection
    public void displaySelection() {
        System.out.println("You have selected:");
        System.out.println("Product: " + this.product);
        System.out.println("Delivery Date: " + this.deliveryDate);
        System.out.println("Delivery Time: " + this.deliveryTime);
    }

    // Show a summary of the order
    public void showOrderSummary() {
        System.out.println("Order Summary for " + this.customerName + ":");
        System.out.println("Product: " + this.product);
        System.out.println("Delivery Date: " + this.deliveryDate);
        System.out.println("Delivery Time: " + this.deliveryTime);
    }

    // Save the order summary to a text file
    public void saveToFile() throws IOException {
        String content = "Order Summary for " + this.customerName + ":\n" +
                         "Product: " + this.product + "\n" +
                         "Delivery Date: " + this.deliveryDate + "\n" +
                         "Delivery Time: " + this.deliveryTime + "\n";
        BufferedWriter writer = new BufferedWriter(new FileWriter("order-summary.txt"));
        writer.write(content);
        writer.close();
        System.out.println("Order summary has been saved to order-summary.txt");
    }
}

// Demo class with the main method
public class Orderfood {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome to the Food Ordering System!");
        System.out.println("Please enter your name to start the order:");
        String name = scanner.nextLine();

        FoodOrder order = new FoodOrder(name);
        order.selectProduct();
        order.selectDeliveryDateTime();
        order.displaySelection();
        order.showOrderSummary();
        order.saveToFile();

        scanner.close();
    }
}
