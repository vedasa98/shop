ShoppingCart
│
├── src
│   ├── CartItem.java
│   ├── ShoppingCart.java
│   ├── Product.java
│   └── Main.java
│
└── README.md


public class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return name + " - $" + price;
    }
}
public class CartItem {
    private Product product;
    private int quantity;

    public CartItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }

    public Product getProduct() {
        return product;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public double getTotalPrice() {
        return product.getPrice() * quantity;
    }

    @Override
    public String toString() {
        return product.getName() + " x " + quantity + " = $" + getTotalPrice();
    }
}
import java.util.ArrayList;
import java.util.List;

public class ShoppingCart {
    private List<CartItem> cartItems;

    public ShoppingCart() {
        cartItems = new ArrayList<>();
    }

    public void addProduct(Product product, int quantity) {
        for (CartItem item : cartItems) {
            if (item.getProduct().getName().equals(product.getName())) {
                item.setQuantity(item.getQuantity() + quantity);
                return;
            }
        }
        cartItems.add(new CartItem(product, quantity));
    }

    public void removeProduct(String productName) {
        cartItems.removeIf(item -> item.getProduct().getName().equals(productName));
    }

    public double calculateTotal() {
        double total = 0;
        for (CartItem item : cartItems) {
            total += item.getTotalPrice();
        }
        return total;
    }

    public void displayCart() {
        System.out.println("Shopping Cart:");
        for (CartItem item : cartItems) {
            System.out.println(item);
        }
        System.out.println("Total: $" + calculateTotal());
    }
}
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        Scanner scanner = new Scanner(System.in);

        Product product1 = new Product("Laptop", 800);
        Product product2 = new Product("Headphones", 50);
        Product product3 = new Product("Keyboard", 30);

        boolean exit = false;

        while (!exit) {
            System.out.println("1. Add Laptop to cart");
            System.out.println("2. Add Headphones to cart");
            System.out.println("3. Add Keyboard to cart");
            System.out.println("4. Remove item from cart");
            System.out.println("5. Checkout");
            System.out.println("6. Exit");

            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    cart.addProduct(product1, 1);
                    break;
                case 2:
                    cart.addProduct(product2, 1);
                    break;
                case 3:
                    cart.addProduct(product3, 1);
                    break;
                case 4:
                    System.out.println("Enter product name to remove: ");
                    scanner.nextLine(); // consume the leftover newline
                    String productName = scanner.nextLine();
                    cart.removeProduct(productName);
                    break;
                case 5:
                    cart.displayCart();
                    System.out.println("Thank you for shopping!");
                    exit = true;
                    break;
                case 6:
                    exit = true;
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        scanner.close();
    }
}
