// project-race-game
//[Project-race.zip](https://github.com/marivoid/project-race-game/files/6217152/Project-race.zip)

import javafx.event.Event;
import javafx.event.EventHandler;
import javafx.scene.Node;
import javafx.scene.input.KeyCode;
import javafx.scene.input.KeyEvent;
import javafx.application.*;
import javafx.event.*;
import javafx.scene.*;
import javafx.scene.image.*;
import javafx.scene.control.*;
import javafx.scene.control.Alert.*;
import javafx.scene.text.*;
import javafx.scene.layout.*;
import javafx.scene.shape.*;
import javafx.stage.*;
import javafx.geometry.*;
import javafx.animation.*;
import java.io.*;
import java.util.*;

/** 
 * Game2DSTARTER with JavaFX and Threads
   Valentines edition
   TODO: car opacity on the white area
*/

public class CarMovement extends Application {
   // Window attributes
   private Stage stage;
   private Scene scene;
   private VBox root;
   
   private boolean upPressed = false;
   private boolean downPressed = false;
   private boolean rightPressed = false;
   private boolean leftPressed = false;
   
   
   private static String[] args;
   
   private final static String ICON_IMAGE="CarSprite1.png";  // file with icon for a racer
   
   private int iconWidth;                       // width (in pixels) of the icon
   private int iconHeight;                      // height (in pixels) or the icon
   private CarRacer racer = null;               // array of racers
   private Image carImage =  null;
   
   private double carSpeed=0;
   private double carSteeringAngle=0;

   private AnimationTimer timer;                // timer to control animation
   
   // main program
   public static void main(String [] _args) {
      args = _args;
      launch(args);
   }
   
   // start() method, called via launch
   public void start(Stage _stage) {
      // stage seteup
      stage = _stage;
      stage.setTitle("Game2D Starter");
      stage.setOnCloseRequest(
         new EventHandler<WindowEvent>() {
            public void handle(WindowEvent evt) {
               System.exit(0);
            }
         });
      
      // root pane
      root = new VBox();
                 
      // create an array of Racers (Panes) and start
      initializeScene();
   }//end of START

   // start the race
   public void initializeScene() {
      
      // Make an icon image to find its size
      try {
         carImage = new Image(new FileInputStream(ICON_IMAGE));
      }
      catch(Exception e) {
         System.out.println("Exception: " + e);
         System.exit(1);
      }
            
      // Get image size
      iconWidth = (int)carImage.getWidth();
      iconHeight = (int)carImage.getHeight();
      
      
      racer = new CarRacer();
      root.getChildren().add(racer);
      root.setId("pane");
      
      // display the window
      scene = new Scene(root, 1200, 700);
      scene.getStylesheets().addAll(this.getClass().getResource("style.css").toExternalForm());
      
            
      
      
      stage.setScene(scene);
      
      stage.show(); 
      
      System.out.println("Starting race...");
      
      // Use an animation to update the screen
      timer = 
         new AnimationTimer() {
            public void handle(long now) {
               racer.update();
            }
         };
      
      // TimerTask to delay start of race for 2 seconds
      TimerTask task = 
         new TimerTask() {
            public void run() { 
               timer.start();
            }
         };
      Timer startTimer = new Timer();
      long delay = 1000L;
      startTimer.schedule(task, delay);
      
              //raceROT+=1;
         
         //MOVEMENT CODE
      scene.setOnKeyPressed(
         e -> {
            if (e.getCode() == KeyCode.A) {
               System.out.println("A was pressed");
               carSteeringAngle = -10;
               leftPressed = true;
            }
            
            if (e.getCode() == KeyCode.D) {
               System.out.println("D was pressed");
               carSteeringAngle = 10;
               rightPressed = true;
            }
            
            if (e.getCode() == KeyCode.S) {
               System.out.println("S was pressed");
               downPressed = true;
            }
            
            if (e.getCode() == KeyCode.W) {
               System.out.println("W was pressed");
               carSpeed=5;
               upPressed = true;
              
            }
         });
       
      scene.setOnKeyReleased(
         e -> {
            if (e.getCode() == KeyCode.A) {
               System.out.println("A was released");
               carSteeringAngle = 0;
            }
            
            if (e.getCode() == KeyCode.D) {
               System.out.println("D was released");
               carSteeringAngle = 0;
            }
            
            if (e.getCode() == KeyCode.S) {
               System.out.println("S was released");
            }
            
            if (e.getCode() == KeyCode.W) {
               carSpeed=0;
               System.out.println("W was released");
               upPressed = true;
              
            }
         });
   
   }
   
   /** 
      Racer creates the race lane (Pane) and the ability to 
      keep itself going (Runnable)
   */
   protected class CarRacer extends Pane {
      private int racePosX=0;          // x position of the racer
      private int racePosY=0;          // x position of the racer
      private int raceROT=0;          // x position of the racer
      private ImageView aPicView;   // a view of the icon ... used to display and move the image
   
      public CarRacer() {
         // Draw the icon for the racer
         aPicView = new ImageView(carImage);
         this.getChildren().add(aPicView);
      }
   
      /** 
         update() method keeps the thread (racer) alive and moving.  
      */
      public void update() {            
      
         double deltaX = carSpeed * Math.cos(raceROT*3.14/180.0);
         racePosX+=deltaX;
         
         double deltaY = carSpeed * Math.sin(raceROT*3.14/180.0);
         racePosY+=deltaY;
         
         double carLength = 5;
         double deltaROT = (carSpeed/carLength)*Math.tan(carSteeringAngle*3.14/180.0) * 30;
         raceROT+=deltaROT;
         
         
        
         //racePosX += (int)(Math.random() * iconWidth / 60);
         //racePosY += (int)(Math.random() * iconWidth / 60);
         aPicView.setTranslateX(racePosX);
         aPicView.setTranslateY(racePosY);
         aPicView.setRotate(raceROT);
            
         //if(racePosX>800) racePosX=0;
         //if(racePosY>500) racePosY=0;
      
      
      
      }  // end update()
   }  // end inner class Racer
   
   
   
} // end class Races
