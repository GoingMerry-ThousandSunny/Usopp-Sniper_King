# Design Pattern- Elements of Reusable Object-Oriented Software

> ### "The best designers will use many design patterns that dovetail and intertwine to produce a whole"
> Erich Gamma 

\
Software design patterns are a huge source of discussion among peers.

Leyt's start our tour, just follow along. But wait, before we start exploration let's first learn what is a design pattern?
## What is Software Design Pattern?
They provide a general reusable solution for the common problems that occur in software design. The pattern typically shows relationships and interactions between classes or objects. The idea is to speed up the development process by providing design paradigms.

> ### **NOTE** : Design pattern represents idea, not a particular implementation.

So Let's Begin.... 
  
 ## 1. Singleton 

The purpose is to create a single instance. Take an example of the calendar; it is a software program that can make only one instance of a class and doesn't allow other cases. It use **getInstance()** to get object.


It's used for logging, thread pool, driver objects, and caching.
 
 ![Singleton pic](https://media.geeksforgeeks.org/wp-content/uploads/SINGLEton.png)

 ### *__Types__* :
 * __Early Instantiation :__
 
 ```C
	public class ABC{
        private static final ABC abc= new ABC();
        private ABC(){
            // Constructor.
        }
        public static ABC getInstance(){
            return abc;
        }
    }
 ```
 > ### **NOTE** : At private static final ABC abc= new ABC(), Early instance will be created at load time. 
  -----
  <br/>

  * __Lazy Instantiation :__

  ```C
  public class ABC{
        private static final instance;
        private ABC(){
            // Constructor.
        }
        public static ABC getInstance(){
            if (instance == null){
                instance= new ABC();
            }
            return instance;
        }
    }
  ```
  > ### **NOTE** : At instance= new ABC(), instance will be created at request time.  
  -----
  <br/>

 ## 2. Decorator : 

It is best when you need add-on class.

![Decorator pic](https://www.tutorialspoint.com/design_pattern/images/decorator_pattern_uml_diagram.jpg)
<br/>

__For example :__

Draw a Shape with red colored borders. 

> ### **NOTE** : The red colored borders doesn't change the object itself, as it's just an add-on on the shape.
<br>
```C
public interface Shape {
    Void draw();
}
```
```C
public class Rectangle implements Shape{
    @Override
    public void draw(){
        System.out.println("Shape: Rectangle");
    }
}
```
```C
public class Circle implements Shape{
    @Override
    public void draw(){
        System.out.println("Shape: Circle");
    }
}
```
```C
public abstract class ShapeDecorator implements Shape{
    protected Shape decoShape;
    
    public ShapeDecorator(Shape decoShape){
        this.decoShape=decoShape;
    }
    public void draw(){
        decoShape.draw();
    }
    
}

```
```C
public class RedShapeDecorator extends ShapeDecorator{
    public RedShapeDecorator(Shape decoShape){
        super(decoShape);
    }
    @Override
    public void draw(){
        decorateShape.draw();
        setRedBorder(decoShape);
    }
    private void setRedBorder(Shape decoShape){
        System.out.println("Border Color: Red");
    } 
}
```
```C
public class Decorator{
    public static void main(String[] ards){
        Shape circle= new Circle();
        
        Shape redCircle= new RedShapeDecorator(new Circle());
        Shape redRectangle= new RedShapeDecorator(new Rectangle());

        System.out.println("Circle with normal border");

        circle.draw();

        System.out.println("Circle with red border");

        redCircle.draw();

        System.out.println("Rectangle with red border");

        redRectangle.draw();

    }
}
```
-----
  <br/>

 ## 3. Command Design Pattern : 

 It intends to  __encapsulate all the data required for performing a given action (command) in an object.__
![Command pic](https://refactoring.guru/images/patterns/content/command/command-en.png?id=80fbadc666cf3b9b1958)
<br/>

 __For example:__

We want to develop a programmable remote which can be used to turn on and off various items like lights and stereo. 

```C
interface Command{
    public void execute();
}
```
```C
class Light{
    public void on(){
        System.out.println("Light is on");
    }
    public void off(){
        System.out.println("Light is off");
    }
}
```
```C
class LightOnCommand implements Command{
    Light light;
    // the constructor is passed which light is going to control
    public LightOnCommand(Light light){
        this.light= light
    }
    public void execute(){
        light.on();
    } 
}
```
```C
class LightOffCommand implements Command{
    Light light;
    public LightOffCommand(Light light){
        this.light= light
    }
    public void execute(){
        light.off();
    } 
}
```
```C
class Stereo{
public void on(){
    System.out.println("Stereo is on);
}
public void off(){
    System.out.println("Stereo is off);
}
public void setCD(){
    System.out.println("Stereo is set"+" for CD input");
}
public void setDVD(){
    System.out.println("Stereo is set"+" for DVD input");
}
public void setRadio(){
    System.out.println("Stereo is set"+" for Radio");
}
public void setVolume(int volume){
    System.out.println("Stereo is set"+" to "+volume);
}
}
```
```C
class StereoOffCommand implements Command{
    Stereo stereo;
    public StereoOffCommand(Stereo stereo){
        this.stereo= stereo;
    }
    public void execute(){
        stereo.off();
    }
    
}
```
```C
public StereoOnWithCDCommand implements COmmand{
    Stereo stereo;
    public StereoOnWithCDCommand(Stereo stereo){
        this.stereo=stereo;
    }
    public void execute(){
        stereo.on();
        stereo.setCD();
        stereo.setVolume(15);
    }
}
```
```C
// Simple remote control with one button.
class SimpleRemoteControl{
    command slot; //only one button
    public SimpleRemoteControl(){}
    public void setCommand(Command command){
        // set the command remote will execute
        slot= command;
    }
    public void buttonWasPressed(){
        slot.execute();
    }
}
```
```C
class RemoteControl{
    public static void main(String[] args){
        SimpleRemoteControl rc= new SimpleRemoteControl();
        Light light= ne Light();
        Stereo stereo= new Stereo();
        // dynamic input
        rc.setCommand(new LightOnCommand(light));
        rc.buttonWasPressed();

        rc.setCommand(new StereoOnWithCDCommand(stereo));
        rc.buttonWasPressed();

        rc.setCommand(new StereoOffWithCDCommand(stereo));
        rc.buttonWasPressed();
    }
}
```
---
<br/>

## 4. Factory Design Pattern : 

It helps to create an object without the user getting exposed to building logic.  

![Factory Pattern Pic](https://stacktraceguru.com/wp-content/uploads/2020/01/creational-pattern-design-1.png)
<br>

__For example :__

There is a vehicle factory, which produces 2
types of vehicles, __Car__ and __Cycle__.
<br>

```C
public enum VehicleType {
    Car, Cycle;
}
```
```C
public abstract class Vehicle {
    public Vehicle(VehicleType type){
        this.type=type;
        arrangeParts();
    }
    public void arrangeParts(){
        // write code.
    }
    protected abstract void construct();
    private VehicleType type= null;
    public VehicleType getType(){
        return type;
    }
    public void setType(VehicleType type){
        this.type= type;
    }
}

```
```C
public class Car extends Vehicle {
    Car() {
        super(VehicleType.Car);
        construct();
    }
    @Override
    protected void construct(){
        System.out.println("Building Car");
        // add accessories.
    }
```
```C
public class Cycle extends Vehicle {
    Cycle() {
       
    protected void construct() {
        System.out.println("Building Cycle");
        // add accessories.
    }
}
```
```C
public class Factory {
    public static BuildVehicle (VehicleType type){
        Vehicle v= null;
        switch(type) {
            case CAR:
                v= new Car();
            case CYCLE:
                v= new Cycle();
            default:
                //throw exception.
                break;
        }
        return v;
    }
}
```
```
public class TestFactory {
    public static void main(String[] args) {
        System.out.println(Factory.BuildVehicle(VehicleType.CAR));
        System.out.println(Factory.BuildVehicle(VehicleType.CYCLE));
    }
}
```
---
<br>

## 5. Observer Pattern

It creates multiple dependencies, i.e, defines a one-to-many dependency between objects so that when the object changes state, all its dependents are notified and updated automatically.
 

![Observer Pattern pic](https://2.bp.blogspot.com/-E4ik7Rk3DGk/WlO4DrleVTI/AAAAAAAAAMI/v-L3z8dcta0UPjcxep7Si6efJe_r7HA5ACLcBGAs/s1600/observer.PNG)

<br>

__Example :__
Message publisher of type __Subject__, with 2 subscribers of type __Observer__, where the publisher will publish message and subscribers will print message.
<br>

```C
public interface Subject{
    public void attach(Observer o);
    public void notifyUpdate(Message m);
}
```
```C
public interface Observer{
    public void update(Message m);
}
```
```C
public class MessagePublisher implements Subject{
    private List<Observer> observers= new ArrayList<>();

    @Override
    public void attach(Observer o){
        observers.add(o);
    }
    @Override
    public void notifyUpdate(Message m){
        for(Observer o: observers){
            o.update(m);
        }
    }
}    
```
```C
public class SubscriberOne implements Observer{
    @Override
    public void update(Message m){
        System.out.println("SubscriberOne"+m.getMessage());
    }
}
```
```C
public class SubscriberTwo implements Observer{
    @Override
    public void update(Message m){
        System.out.println("SubscriberTwo"+m.getMessage());
    }
}
```
```C
public class Message{
    final String message;
    public Message(String m){
        this.message=m;
    }
    public String getMessage(){
        return message;
    }
}
```
```C
public class Main{
    public static void main(String[] ards){
        SubscriberOne s1= new SubscriberOne();
        SubscriberTwo s2= new SubscriberTwo();

        MessagePublisher mp= new MessagePublisher();

        mp.attach(s1);
        mp.attach(s2);
        mp.notifyUpdate(new Message("First Message")); 

        mp.attach(s1);
        mp.attach(s2);
        mp.notifyUpdate(new Message("Second Message"));
    }
}
```
-----------
<br>

## Conclusion :

Software design patterns help you as a programmer to create robust architecture. 
For that you need to have the right knowledge of it's usage.

## References :

<br>

* https://www.upgrad.com/blog/software-design-patterns/
<br>

* https://www.geeksforgeeks.org/software-design-patterns/
