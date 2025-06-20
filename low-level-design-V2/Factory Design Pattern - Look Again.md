task1: EmailNotification respective Factory class
```java
// sending Notification
public class EmailNotification {
	public void send(String message){
		System.out.println("Sending an Email notification:" + message);
	}
}

// DriverClass
public class DriverClass {
	public void sendNotification(String message){
		EmailNotification email = new EmailNotification();
		email.send("Message");
	}
}
```

task2: supports SMS Notification
```java
// Email Notification
public class EmailNotification {
	public void send(String message){
		System.out.println("Sending an Email notification:" + message);
	}
}

// SMS Notification
public class SMSNotification {
	public void send(String message){
		System.out.println("Sending an SMS notification:" + message);
	}
}

// Driver Class
public class DriverClass {
	public void sendNotification(String type, String message){
		if (type.equals("EMAIL")) {
			EmailNotification email = new EmailNotification();
			email.send(message);
		} else if (type.equals("SMS")){
			SMSNotification sms = new SMSNotification();
			sms.send(message);
		}
	}
}
```

task3: supports PUSH Notification also
```java
// Email Notification
public class EmailNotification {
	public void send(String message){
		System.out.println("Sending an Email notification:" + message);
	}
}

// SMS Notification
public class SMSNotification {
	public void send(String message){
		System.out.println("Sending a SMS notification:" + message);
	}
}

// PUSH Notification
public class PushNotification{
	public void send(String message){
		System.out.println("Sending a PUSH notification" + message);
	}
}

// Driver Class
public class DriverClass {
	public void sendNotification(String type, String message){
		if (type.equals("EMAIL")) {
			EmailNotification email = new EmailNotification();
			email.send(message);
		} else if (type.equals("SMS")){
			SMSNotification sms = new SMSNotification();
			sms.send(message);
		} else if (type.equals("PUSH")){
			PushNotification push = new pushNotification();
			push.send(message);
		}
	}
}
```

task4: Adding Slack alerts, Then WhatsApp
```java
// EmailNotification.send(String message);
// SMSNotification.send(String message);
// PushNotification.send(String message);
// WhatsAppNotification.send(String message);
// SlackNotification.send(String message);

// Driver class starts bloating with if-else loop: 
public class DriverClass {
	public void sendNotification(String type, String message){
		if (type.equals("EMAIL")) {
			EmailNotification email = new EmailNotification();
			email.send(message);
		} else if (type.equals("SMS")){
			SMSNotification sms = new SMSNotification();
			sms.send(message);
		} else if (type.equals("PUSH")){
			PushNotification push = new pushNotification();
			push.send(message);
		} else if (type.equals("SLACK")){
			SlackNotification slk = new SlackNotification();
			slk.send(message);	
		} else if (type.equals("WhatsApp")){
			WhatsAppNotification wa = new WhatsAppNotification();
			wa.send(message);
		}
	}
}
```

###### Problem:

> Everytime new Notification Channel get's added FactoryClass.sendNotification should be rewritten

> The if-else code starts bloating, Again it should be retested.

>It violates key design principles, especially the Open/Closed Principle - the idea that classes should be open for extension but closed for modification

>In a team for DriverClass.sendNotification() might occur merge conflicts, if devs are parallely changing the file based on requirements.


###### Clean It Up with a Simple Factory
```java
// Notification.send(String message) : Interface
// EmailNotification.send(String message);
// SMSNotification.send(String message);
// PushNotification.send(String message);
// WhatsAppNotification.send(String message);
// SlackNotification.send(String message);

// FactoryClass : Code refactored to Factory class
public class Factory {
	public static Notification createNotification(String type){
		if (type.equals("EMAIL"))
			return new EmailNotification();
		else if (type.equals("SMS"))	
			return new SMSNotification();
		else if (type.equals("PUSH"))	
			return pushNotification();
		else if (type.equals("SLACK"))
			return new SlackNotification();
		else if (type.equals("WhatsApp"))	
			return new WhatsAppNotification();
	}
}

// DriverClass
// get suitable Notification Instance & sends message. 
public class DriverClass {
	public void sendNotification(String type, String message){
		Notification notification = Factory.createNotification(type);
		notification.send(message);
	}
}
```

> Still the problem shifted to Factory Class, below is the idea

```
if type == "EMAIL": return new EmailNotification();
EmailNotificationCreator.create() : return new EmailNotification()
i.e. Object Creation is taken care of separate Classes.
```


```java
// FactoryClass : Code refactored to Factory class
public class Factory {
	public static Notification createNotification(String type){
		if (type.equals("EMAIL"))
			return new EmailNotification();
		else if (type.equals("SMS"))	
			return new SMSNotification();
		else if (type.equals("PUSH"))	
			return pushNotification();
		else if (type.equals("SLACK"))
			return new SlackNotification();
		else if (type.equals("WhatsApp"))	
			return new WhatsAppNotification();
	}
}

// EmailNotificationCreator.createProduct() : return new EmailNotification();
// SMSNotificationCreator.createProduct() : return new SMSNotification();
// PushNotificationCreator.createProduct() : return new PushNotification();
// SlackNotificationCreator.createProduct() : return new SlackNotification();
// WhatsAppNotificationCreator.createProduct() : return new WhatsAppNotification();
```

```java
// class Factory.createNofication() got refactored below.

public abstract class Creator {
	public abstract Notification createNotification();
	public void send(String message){
		Notification notification = createNotification();
		notification.send(message);
	}
}

// EmailNotificationCreator.createProduct() : EmailNotification
// SMSNotificationCreator.createProduct() : SMSNotification
// PushNotificationCreator.createProduct() : PushNotification
// SlackNotificationCreator.createProduct() :  SlackNotification
// WhatsAppNotificationCreator.createProduct() :  WhatsAppNotification
```

```java
// In short if any new Product got added, it's creator will also get's added. You can use new InstanceOfCreator & executes Creator.action(). To execute Product.actions()
class Driver{
	Creator creator = new WhatsAppNotificationCreator();
	creator.send(message);
}
```

```java
// With old setup, you'd have to:
1. Mofify your Factory
2. Add new if-else or switch cases
3. Risk breaking existing logic

// With the Factory Method pattern, you simply:
1. Create a new SlackNotificationCreator();
2. Implement action Creator.send(message)
3. Done
```

###### Solution:
```java
// Finalized Version:

// Product

// Creator

// Driver
```

###### Granular Solution:
```java
// Finalized Version: Granular Solution

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

// Creator ..> Notification
public abstract class Creator {
	// Capture Product
	public abstract Notification createNotification();

	// Perform Actions
	public void action(String message){
		Notification notification = createNotification();
		notification.send(message);
	}
}

public class EmailNotificationCreator extends Creator {
	@Override
	public Notification createNotification() {
		return new EmailNotification();
	}
}

// Driver
public class Driver {
	public static void main(String[] args){
		Creator creator;

		//send Email
		creator = new EmailNotificationCreator();
		creator.action("Email Notification Sent!!!");
	}
}
```


ProductionCode: design-patterns/java/factory
```java
// complete the production ready code present in design-patterns.java.factory

// Product:
// Notification (Interface)
// EmailNotification.send(String)
// PushNotification.send(String)

// WithoutFactoryPattern
// NotificationServiceNaive

// Creator:
// NotificationCreator (abstract Class); 
// EmailNotificationCreator.createNotification();

// DriverClass
// FactoryMethodDemo.main()
```
