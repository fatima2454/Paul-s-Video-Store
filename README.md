# Paul-s-Video-Store
I will fix this later on

Main: 

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        VideoManager videoManager = new VideoManager();
        CustomerManager customerManager = new CustomerManager();

        // Movie list setup finally i'm done it's 3 am
        videoManager.addVideo("Midsommar", "031398306870", false);
        videoManager.addVideo("Iron Man", "799491431348", false);
        videoManager.addVideo("The Little Mermaid", "786936771688", false);
        videoManager.addVideo("Mulan", "786936832785", false);
        videoManager.addVideo("Cinderella", "786936825800", false);
        videoManager.addVideo("Spongebob", "032429303837", false);
        videoManager.addVideo("The Simpsons", "024543484387", false);
        videoManager.addVideo("Hunger Games", "031398181538", false);
        videoManager.addVideo("The Rugrats", "032429041241", false);
        videoManager.addVideo("Jaws", "743167382175", false);
        videoManager.addVideo("Bee Movie", "191329061466", false);
        videoManager.addVideo("Hotel Transylvania", "043396413733", false);
        videoManager.addVideo("Tom and Jerry", "0031398306871", false);
      
//I added the false thing smith becuase when I kepy running it it kept compiling like wierd outputs and putting flase would keep the output clean and focused on more important information. idk if that makes sense but it worked and I can explain it to you in the morning. 
      
        boolean isRunning = true;
        while (isRunning) {
            displayMenu();
            String choice = scanner.nextLine();

            switch (choice) { //options we have avaibale rn just choose one 
                case "1":
                    addVideo(scanner, videoManager);
                    break;
                case "2":
                    findVideo(scanner, videoManager);
                    break;
                case "3":
                    videoManager.listVideos();
                    break;
                case "4":
                    addCustomer(scanner, customerManager);
                    break;
                case "5":
                    rentVideo(scanner, videoManager, customerManager);
                    break;
                case "6":
                    returnVideo(scanner, videoManager, customerManager);
                    break;
                case "7":
                    customerManager.listAllCustomers();
                    break;
                case "8":
                    isRunning = false;
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }

        scanner.close();
    }

    private static void displayMenu() {//pick a number 1-8 yay fun game 
        System.out.println("Menu:\n");
        System.out.println("Add Video - 1");
        System.out.println("Look for Video - 2");
        System.out.println("All Videos - 3");
        System.out.println("Add Customer - 4");
        System.out.println("Let customer Rent - 5");
        System.out.println("Let customer Return - 6");
        System.out.println("All Customers - 7");
        System.out.println("Exit - 8");
        
    }

    public static void addVideo(Scanner scanner, VideoManager videoManager) {
        System.out.println("Enter Video Title:");
        String title = scanner.nextLine();
        System.out.println("Enter Video Barcode (12 digits):");
        String barcode = scanner.nextLine();
        videoManager.addVideo(title, barcode);
    }

    public static void findVideo(Scanner scanner, VideoManager videoManager) {
        System.out.println("Enter Video Title:");
        String title = scanner.nextLine();
        Video video = findVideoByTitle(videoManager, title);
            if (video != null) {
                System.out.println(video);
                if (video.isRented()) {
                    Customer rentingCustomer = video.getCurrentRentedCustomer();
                    System.out.println("Rented by: " + rentingCustomer.getFullName() + " (Phone: " + rentingCustomer.getPhoneNumber() + ")");
                } else {
                    System.out.println("The video is currently not rented.");
                }
            } else {
                System.out.println("Video does not exist.");
            }
        }

    public static void addCustomer(Scanner scanner, CustomerManager customerManager) {
        System.out.println("Enter Customer First Name:");
        String firstName = scanner.nextLine();
        System.out.println("Enter Customer Last Name:");
        String lastName = scanner.nextLine();
        System.out.println("Enter Customer Phone Number:");
        String phoneNumber = scanner.nextLine();
        customerManager.addCustomer(firstName, lastName, phoneNumber);
    }

    public static void rentVideo(Scanner scanner, VideoManager videoManager, CustomerManager customerManager) {
        System.out.println("Enter Customer Phone Number:");
        String phoneNumber = scanner.nextLine();

        Customer customer = findCustomerByPhoneNumber(customerManager, phoneNumber);
        if (customer == null) {
            System.out.println("Customer not found.");
            return;
        }

        System.out.println("Enter Video Barcode:");
        String barcode = scanner.nextLine();

        Video video = findVideoByBarcode(videoManager, barcode);
        if (video == null || video.isRented()) {
            System.out.println("Video not available.");
            return;
        }

        customer.rentVideo(video);
        System.out.println("Video rented successfully to " + customer.getFullName());
    }

    public static void returnVideo(Scanner scanner, VideoManager videoManager, CustomerManager customerManager) {
        System.out.println("Enter Customer Phone Number:");
        String phoneNumber = scanner.nextLine();

        Customer customer = findCustomerByPhoneNumber(customerManager, phoneNumber);
        if (customer == null) {
            System.out.println("Customer not found.");
            return;
        }

        System.out.println("Enter Video Barcode:");
        String barcode = scanner.nextLine();

        Video video = findVideoByBarcode(videoManager, barcode);
        if (video == null || !video.isRented()) {
            System.out.println("Video is not currently rented.");
            return;
        }

        customer.returnVideo(video);
        System.out.println("Video returned successfully.");
    }

    public static Customer findCustomerByPhoneNumber(CustomerManager customerManager, String phoneNumber) {
        for (int i = 0; i < customerManager.customerSize; i++) {
            if (customerManager.customerData[i].getPhoneNumber().equals(phoneNumber)) {
                return customerManager.customerData[i];
            }
        }
        return null;
    }
  
    public static Video findVideoByBarcode(VideoManager videoManager, String barcode) {
        for (int i = 0; i < videoManager.videoSize; i++) {
            if (videoManager.videoStorage[i].getBarcode().equals(barcode)) {
                return videoManager.videoStorage[i];
            }
        }
        return null;
    }
    public static Video findVideoByTitle(VideoManager videoManager, String title) {
        for (int i = 0; i < videoManager.videoSize; i++) {
            if (videoManager.videoStorage[i].getTitle().equalsIgnoreCase(title)) {
                return videoManager.videoStorage[i];
            }
        }
        return null;
    }
}
