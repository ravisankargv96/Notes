### Factory Design Pattern
##### Problem:
```java

// Product
public interface Notification {
    public void send(String message);
}

public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}

public class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending push notification: " + message);
    }
}

public class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}


// Naive Solution : Driver Class
public class NotificationServiceNaive {
    public void sendNotification(String type, String message) {
        if (type.equals("EMAIL")) {
            EmailNotification email = new EmailNotification();
            email.send(message);
        } else if (type.equals("SMS")) {
            SMSNotification sms = new SMSNotification();
            sms.send(message);
        } else if (type.equals("Push")) {
            PushNotification push = new PushNotification();
            push.send(message);
        }
    }    
}
```

###### Intermediate Solution
```java
// Product
public interface Notification {
    public void send(String message);
}

public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}

public class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending push notification: " + message);
    }
}

public class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}


// Stage 2 Solution
public class SimpleNotificationFactory {
    public static Notification createNotification(String type) {
        return switch (type) {
            case "EMAIL" -> new EmailNotification();
            case "SMS" -> new SMSNotification();
            case "PUSH" -> new PushNotification();
            default -> throw new IllegalArgumentException("Unknown type");
        };
    }    
}



// Driver Class : uses SimpleNotificationFactory Instance.
```

###### Full Solution:
```java
// Factory Design Pattern : Solution

// Product
public interface Notification {
    public void send(String message);
}

public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}

public class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending push notification: " + message);
    }
}

public class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}


// Creator
public abstract class NotificationCreator {
    // Factory Method
    public abstract Notification createNotification();
    
    // Common logic using the factory method
    public void send(String message) {
        Notification notification = createNotification();
        notification.send(message);
    }
}

public class EmailNotificationCreator extends NotificationCreator {
    @Override
    public Notification createNotification() {
        return new EmailNotification();
    }
}

public class PushNotificationCreator extends NotificationCreator {
    @Override
    public Notification createNotification() {
        return new PushNotification();
    }
}

public class SMSNotificationCreator extends NotificationCreator {
    @Override
    public Notification createNotification() {
        return new SMSNotification();
    }
}




// Driver
public class FactoryMethodDemo {
    public static void main(String[] args) {
        NotificationCreator creator;

        // Send Email
        creator = new EmailNotificationCreator();
        creator.send("Welcome to our platform!");

        // Send SMS
        creator = new SMSNotificationCreator();
        creator.send("Your OTP is 123456");

        // Send Push Notification
        creator = new PushNotificationCreator();
        creator.send("You have a new follower!");
    }    
}
```

### Abstract Factory Design Pattern

###### Code:
```java

// Product 1 : Button
public interface Button {
    void paint();
    void onClick();
}

public class MacOSButton implements Button {
    @Override
    public void paint() {
        System.out.println("Painting a macOS-style button.");
    }

    @Override
    public void onClick() {
        System.out.println("MacOS button clicked.");
    }
}

public class WindowsButton implements Button {
    @Override
    public void paint() {
        System.out.println("Painting a Windows-style button.");
    }

    @Override
    public void onClick() {
        System.out.println("Windows button clicked.");
    }
}


// Product 2 : Checkbox
public interface Checkbox {
    void paint();
    void onSelect();
}

public class MacOSCheckbox implements Checkbox {
    @Override
    public void paint() {
        System.out.println("Painting a macOS-style checkbox.");
    }

    @Override
    public void onSelect() {
        System.out.println("MacOS checkbox selected.");
    }
}

public class WindowsCheckbox implements Checkbox {
    @Override
    public void paint() {
        System.out.println("Painting a Windows-style checkbox.");
    }

    @Override
    public void onSelect() {
        System.out.println("Windows checkbox selected.");
    }
}

// Factory 1: ProductsCreator
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

public class MacOSFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacOSButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacOSCheckbox();
    }
}


public class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

// Application Code
public class Application {
    private final Button button;
    private final Checkbox checkbox;

    public Application(GUIFactory factory) {
        this.button = factory.createButton();
        this.checkbox = factory.createCheckbox();
    }

    public void renderUI() {
        button.paint();
        checkbox.paint();
    }
}


// Driver Code
public class AppLauncher {
    public static void main(String[] args) {
        // Simulate platform detection
        String os = System.getProperty("os.name").toLowerCase();
        GUIFactory factory;

        if (os.contains("mac")) {
            factory = new MacOSFactory();
        } else {
            factory = new WindowsFactory();
        }

        Application app = new Application(factory);
        app.renderUI();
    }
}
```

###### Example2:
```java
/**
// product1
Sole (I)
	BumpySole
	FlatSole
	ThinSole

// product2
ShoeLace(I):
	RoundShoeLace
	TapeShoeLace

// Factory.methods : createProducts()
ShoeFactory(I): .createShoeSole() : Sole; .createShoeLace() : ShoeLace

	FormalShoeFactory
	SportsShoeFactory
	CasualShoeFactory

Shoe

// DriverCode:
ShoeManufacture --> ShoeFactoryMaker --> ShoeFactory

	
*/

/**
Relations:

// product1:
ThinSole --|> Sole
FlatSole --|> Sole
BumpySole --|> Sole


// product2:
TapeShoeLace --|> ShoeLace
RoundShoeLace --|> ShoeLace

// Factory
FormalShoeFactory --|> ShoeFactory
SportsShoeFactory --|> ShoeFactory
CasualShoeFactory --|> ShoeFactory


ShoeFactory ..> Sole
ShoeFactory ..> ShoeLace



// Driver
ShoeManufacture o--> ShoeFactoryMaker --> ShoeFactory
*/
```

