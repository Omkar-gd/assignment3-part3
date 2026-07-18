# Assignment 3 – Part 3
# 1. High-Level Design (HLD)
## Modules / Components
1. Notification Generation Module
   - Creates notifications when a new assignment is posted or an enrollment status changes.
2. Notification Delivery Module
   - Sends notifications to students through email, SMS, or in-app notifications.
3. User Preferences Module
   - Stores the notification preferences of each student (email, SMS, app notification).
4. Database
   - Stores student details, notification history, and user preferences.

## Layered Architecture
### Presentation Layer
- Displays notifications to users.
- Accepts user requests to view or update notification settings.

### Business Layer
- Processes notification requests.
- Checks user preferences.
- Creates notification messages.

### Data Access Layer
- Retrieves and updates data from the database.
- Saves notification records.

### Database Layer
- Stores student information.
- Stores notification history.
- Stores notification preferences.

## Notification Flow
1. A lecturer posts a new assignment or an enrollment status changes.
2. The Presentation Layer receives the request.
3. The Business Layer creates the notification and checks the student's preferences.
4. The Data Access Layer stores the notification in the database.
5. The Notification Delivery Module sends the notification through the selected channel (Email, SMS, or App).
6. The student receives the notification successfully.



# 2. Architectural Style Choice
## Chosen Architecture
**Event-Driven Architecture**

### Why Event-Driven?
The notification service should send notifications immediately whenever an event occurs, such as a new assignment being posted or an enrollment status changing.

### Advantages
1. Notifications are sent instantly when an event occurs.
2. The notification service is independent from the course and enrollment system, making maintenance easier.
3. It can easily handle a large number of notifications as the number of users grows.

### Challenges
1. Managing events and debugging problems can be more difficult.
2. If the event system fails, some notifications may be delayed until the service recovers.


# 3. Low-Level Design (LLD)
## Class 1: Notification
### Attributes
- notificationId : int
- studentId : int
- message : String
- type : String
- status : String
### Methods
- createNotification(studentId: int, message: String, type: String) : Notification
- markAsRead(notificationId: int) : boolean
- getNotification() : String
### SOLID Principle
**Single Responsibility Principle (SRP):**  
The Notification class is responsible only for creating and managing notification data.

## Class 2: NotificationDispatcher
### Attributes
- dispatcherId : int
- channel : String
### Methods
- sendNotification(notification: Notification) : boolean
- selectChannel(channel: String) : void
### SOLID Principle
**Open/Closed Principle (OCP):**  
The NotificationDispatcher can support new delivery channels (such as Email, SMS, or Push Notification) without modifying the existing code.

## Interface: NotificationService
### Method Signatures
- send(notification: Notification) : boolean
- getChannel() : String
### Implementation Classes
1. EmailNotificationService
2. SMSNotificationService
### Flexibility
Both EmailNotificationService and SMSNotificationService implement the NotificationService interface. This allows the system to switch between different notification channels without changing the NotificationDispatcher class. New channels, such as Push Notification, can also be added easily by implementing the same interface.




# 4. Scalability Plan
## Expected Bottleneck
The main bottleneck is the large number of notifications sent during peak times, such as assignment deadlines or enrollment periods.

## Scaling Choice
**Horizontal Scaling**

### Justification
Horizontal scaling adds more servers instead of increasing the power of one server. This helps the notification service handle many users at the same time and improves availability.

## Elasticity Policy
- **Add resources:** Automatically add more servers when CPU usage is above 70% or when the notification queue becomes too large.
- **Remove resources:** Remove extra servers when CPU usage stays below 30% and the notification queue is small.

## Load Balancing Algorithm
**Round Robin**

### Justification
Round Robin distributes notification requests equally among all available servers. Since notification requests are similar in size, this method provides balanced workload distribution and good performance.



# 5. Cloud Deployment Recommendation
## Recommended Cloud Model
**Public Cloud**

### Why Public Cloud?
A public cloud is suitable because it is cost-effective, easy to deploy, and can automatically scale as the number of users increases. It is a good choice for a university notification system.

## Recommended Cloud Service Model
**Platform as a Service (PaaS)**

### Justification
PaaS provides a platform for developing, testing, and deploying applications without managing servers or hardware. This allows developers to focus on the notification application while the cloud provider handles the infrastructure.

## Security Measures
1. **Authentication and Authorization** – Only authorized users can access the system.
2. **Data Encryption** – Encrypt notification data during storage and transmission.
3. **Regular Backups** – Backup the database regularly to prevent data loss.

