import ddf.minim.*;
/*Robert Fratila
 Submitted to: Mr.Silvello
 Date: January 10, 2014
 Purpose: This is the culmination of everything I learned as well as a few topics searched online. Immerse yourself into my game. 
 */

boolean run3D, run2D;    //attributes that determine what version of the game the user wants to play

String name;             //holds onto the players name
float x, y, z;           //position of the player variables
float planeSpeed;        //the plane speed at which the plane moves
float changeSR;          //this is for determining where the path will spawn
boolean bankRight, bankLeft, bankDown, bankUp;//holds onto information regarding which way the player wants to move
String stage;            //holds onto the current game state
String dependingHighscore;//knows which highscore is to be compared
byte caveLastSlot;       //knows the last position of a slot

//arrays
Obstacles[] dangerObstacles;   //array of the class obstacles
Path [] cave;                  //array for keeping track of the path for the 2d version
Star[] stars;                  //array of the class stars
Bullet[] bullets;              //bullet array
Gravity[] satellites;          //stars moving past the player (aesthetic)
Ship rocket2D;                 //the players 2d ship
String[] player;                //the 3d player name for highscores
int[] playerScore;             //the 3d player highscores
String[] player2D;             //the 2d player name for highscores
int[] playerScore2D;           //the 2d player highscores
Bullet2D [] shoot;             //the 2d bullets
Stalactite wall;               //keeps track of the stalactite (top) in the 2d game
Stalagmite bottom;             //keeps track of the stalagmite (bottom) in the 2d game
Ammo pickup;                   //keeps track of the ammo pickup orb that the player can get

//progression variables
float obsAdvance;              //keeps track of the amount of asteroids on the screen 
int time;                      //keeps track of the time
float speed;                   //keeps track of the speed of the asteroids

//health variables
byte health;                   //the players health variable
color bar;                     //the colour of the players health bar
float recWidth;                //the width of the players health bar

float xGun, yGun, zGun;        //the position of the ammo pickup in the 3d game
byte ammunition;               //the variable that keeps track of how much ammunition the player has left
int ammoLeft;                  //this keeps track of how much ammo the player can pickup
float gunDistance;             //holds the distance between the ammo pickup and the player ship is in the 3d game
boolean shake;                 //a variable that gives the effect that your ship hit something
int score;                     //holds on to the score that the player has. It is common between both the 3d and 2d games
float timeToStart;             //keeps track of the countdown in the 2d game

float wave;                    //this is the wave effect in the background of the menus
float pointPos;                //keeps track of the individual positions of each ball

color menu3D, menu2D, menuInstructions, menuHighscore, back, ok, music;//these variables keep track of the colour of each button
boolean info3D, info2D;        //these keep track of which selection the mouse is on to give more information about the game mode
boolean positionOnce;          //this is meant to initially place the ammo in 3d game once
byte traverse;                 //this is the speed at which everything moves in the 2d game mode

//variables that hold information for different fonts used in the game
PFont smallFont;               
PFont font;
PFont medFont;
PFont huge;

//variables to move the title in the main menu
int moveTitle;
int increments;

boolean musicPlay;

//used only for sound
Minim minim;
AudioPlayer fire;
AudioPlayer explodeSound;
AudioPlayer hit;
AudioPlayer powerup;
AudioPlayer backgroundMusic;
//*****************************************************************************************************************************
//initializes the fundamental variables for both game modes
void setup()
{    
    size(800, 600, OPENGL);                   //calls the 3d render and sets the size of the form

    moveTitle = width/2;                      //places the initial title position
    increments = 1;                           //sets the speed at which the title moves by
    //initializes the font variations
    font = createFont("Stencil", 30);
    smallFont = createFont("Cambria", 12);
    medFont = createFont("Cambria", 20);
    huge = createFont("Cambria", 200);

    //sets the string variables to nothing so that it does not crash
    name = "";
    stage = "";
    dependingHighscore = "";
    wave = 0;

    //the following block of code reads the txt files that is embedded within the file location and sets all highscore variables to what they have always been. This means that the program will always remember the scores from past games
    player = new String[3];
    playerScore = new int[3];
    String players3D[] = loadStrings("player3D.txt");      //reads the 3d player name file
    for (byte i = 0; i < players3D.length; i++)            //sets the 3d player names
    {
        player[i] = players3D[i];
    }
    String p3[] = loadStrings("score3D.txt");              //reads the 3d player score file
    for (byte i = 0; i < p3.length; i++)                   //sets the 3d player scores
    {
        playerScore[i] = Integer.parseInt(p3[i]);
    }

    player2D = new String[3];
    playerScore2D = new int[3];

    String players2D[] = loadStrings("player2D.txt");      //reads the 2d player name file
    for (byte i = 0; i < players2D.length; i++)            //sets the 2d player names
    {
        player2D[i] = players2D[i];
    }

    String p2[] = loadStrings("score2D.txt");              //reads the 2d player score file
    for (byte i = 0; i < p3.length; i++)                   //sets the 2d player scores
    {
        playerScore2D[i] = Integer.parseInt(p2[i]);
    }

    //initializes the button colours
    menu3D = color(250, 194, 73);
    menu2D = color(250, 194, 73); 
    menuInstructions =color(250, 194, 73); 
    menuHighscore = color(250, 194, 73);
    back = color(255, 121, 249);
    ok = color(2, 234, 221);
    music = color(38, 255, 28);

    //for 2d
    rocket2D = new Ship();

    satellites = new Gravity[100];                    //sets the stars in the background
    for (int i = 0; i < satellites.length;i++)        //creates 100 new stars
    {
        satellites[i] = new Gravity();
    }
    shoot = new Bullet2D[31];                         //sets the bullets
    for (byte i = 0; i < shoot.length; i++)           //initializes the bullets to be used
    {
        shoot[i] = new Bullet2D();
    }


    //for 3d
    //draws stars in the background
    stars = new Star[700];                            //initializes the stars used for the 3d mode
    for (int i = 0; i < stars.length;i++)             
    {
        stars[i] = new Star();
    }
    bullets = new Bullet[31];                         //initializes 31 slots in the bullet array
    for (byte i = 0; i < bullets.length;i++)
    {
        bullets[i] = new Bullet();
    }

    minim = new Minim(this);
    fire = minim.loadFile("newFire.mp3");
    explodeSound = minim.loadFile("newExplosion.mp3");
    hit = minim.loadFile("newHit.mp3");
    powerup = minim.loadFile("pickup.mp3");
    backgroundMusic = minim.loadFile("space.mp3");
    musicPlay = true;
    backgroundMusic.loop();
}

void draw ()
{
    if (run3D == false && run2D == false)
    {       
        newBackground();                               //runs the dynamic background
        colorMode(RGB);
        strokeWeight(3);
        stroke(255);
        if (stage == "score input")                    //displayed when the player is meant to input their name
            highScoreInput();

        else if (stage == "show highscore")            //displays the highscore when the player chooses to do so
            displayHighScore();

        else if (stage == "instructions")              //displays the instructions
            displayInstructions();

        else                                           //shows the player the main menu when nothing else is running
        displayMenu();
    }
    else
        noStroke();

    //will run if the 3d version is chosen
    if (run3D == true)
    {   

        background (3, 2, 18);                            //refreshes the background and colour
        time = second();                                  //keeps time based off of the clock on the computer
        lights();                                         //gives 3d depth to all 3d objects

            //activates when you hit an obstacle to make it look like the ship is damaged
        if (shake == true)
        {
            camera(random(x-100, x+100), y - 100, 500, x/2 + 270, y/1.4 + 150, 0, 0, 1, 0);
            if (millis()%9 == 0)
            {
                shake = false;                            //turns of the shake
                determineColourOfHealthBar();             //figures out what the players current health is
            }
        }

        else                                              //return to normal camera angle
        camera(x, y - 75, 500, x/2 + 270, y/1.4 + 150, 0, 0, 1, 0);//the camera angle is positioned behind the player and follows wherever they go       

        pushMatrix();                //moves the 2d grid in 3d space; applies to all pushMatrix() functions
        controlBounds();             //controls the players' boundaries so they can't escape the playing field        

        gameProgression();           //will increase the speed of asteroids and quantity

        drawObstacles();             //controls obstacles
        movePlane();                 //updates planes position    
        drawPlane();                 //draws the plane        
        popMatrix();                 //brings the grid back for the other objects to use

        pushMatrix();
        displayBullets();            //will only show the bullets when the player has ammunition
        popMatrix();

        starBackground();            //draws the stars in the background
        HUD();                       //displays the current information to the player
        score++;                     //the score will increase
    }

    //code runs if the player chose the 2d mode of the game
    else if (run2D == true)
    {   
        background(0);
        drawGalaxy();                    //draws the stars in the background
        noStroke();      
        displayPath();                   //displays the path the player follows
        HUD2D();                         //displays the current info to the player

        if (timeToStart >= 3)            
        {
            progress();                  //makes the game get progressively get harder
            display2DBullets();          //displays the bullets when needed
            rocket2D.move();             //moves the players ship
            rocket2D.collision();        //checks any collision with the ship
            score++;                     //increases score
        }     
        else 
        {
            countdown();                 //does a count down
        }
        rocket2D.draw();                 //draws the players ship
    }
}

//initializes attributes for 3d gameplay
void threeDSetup()
{       
    //initializes original plane position
    x = 500;
    y = 500;
    z = 0; 
    bar = color( 58, 255, 7);
    shake = false;
    //used to draw the gun orb
    positionOnce = true;
    planeSpeed = 10;
    health = 100;
    //responsible for health management
    ammunition = 0;
    score = 0;
    recWidth = 150;

    //handles the obstacles             
    obsAdvance = 30;

    dangerObstacles = new Obstacles [300];//the amount of balls made
    for (int i = 0; i < dangerObstacles.length; i++)
    {
        dangerObstacles[i] = new Obstacles();
    }

    for (int i = 0; i < bullets.length; i++)
    {
        bullets[i].fired = false;
    }
}

void twoDSetup()
{
    timeToStart = 0;
    planeSpeed = 3;
    x = 130;
    y = 200;
    changeSR = 0;
    caveLastSlot = 31;
    score = 0;
    health = 100;
    stage = "";
    ammunition = 10;    
    traverse = 3;
    bar = color(58, 255, 7);   

    pickup = new Ammo();
    wall = new Stalactite();
    bottom = new Stalagmite();
    for (byte i = 0; i < shoot.length; i++)            //sets the bullets to be used again
    {
        shoot[i].shot = false;
    }
    cave = new Path[32];                
    for (byte i = 0; i < cave.length; i++)             //sets the start path and then begins the alternating path
    {
        if (i == 0)
            cave[i] = new Path(0, height/2);
        else
            cave[i] = new Path(cave[i-1].xPos + 26, height/2);
    }
    backgroundMusic = minim.loadFile("space.mp3");
}

//used for the countdown before the game begins in the 2d game mode
void countdown()
{
    textFont(font);   
    timeToStart += 0.02;            
    fill(150, 234, 95, 240);
    rect(width/2, height/2 - 10, 120, 50, 20);
    fill(18, 0, 234);

    if (byte(4 - timeToStart) >2)
        text("Ready", width/2, height/2 );

    else if (byte(4 - timeToStart) > 1)
        text("Set", width/2, height/2 );

    else
        text("GO!", width/2, height/2);
}
//creates a cool new dynamic background that is nice to look at
void newBackground()
{
    noStroke();
    pointPos = 0;
    fill(0, 25);
    rect(0, 0, 1700, 1200);

    //draws circles until the width of the screen is takin up
    while (pointPos < width)
    {

        colorMode(HSB);
        fill(pointPos/6, 100, 100);        
        ellipse(pointPos, height * noise(pointPos / 100, wave), 10, 10);   //uses a noise function to keep the objects moving         
        pointPos = pointPos + 10;
    }
    wave = wave + 0.04;
}
void displayPath()
{
    if (timeToStart >= 3)
    {
        for (byte i = 0; i < cave.length; i++)        //moves the path
        {
            cave[i].move();
        }
        for (byte i = 0; i < cave.length; i++)        //checks for collision to keep the path moving
        {
            cave[i].collision();
        }
    }
    for (byte i = 0; i < cave.length; i++)            //draws the path
    {
        cave[i].draw();
    }
}

//displays the menu to the player
void displayMenu()
{  
    //only used to draw the image while at the menu to reduce uneccessary processing
    if (run3D == false && run2D == false && stage == "")
    {               
        rectMode(CENTER);
        textAlign(CENTER);
        fill(music);
        rect(700, 550, 60, 40, 10);
        fill(255);

        textFont(medFont);  
        text("Programmed by Robert Fratila", 150, 580);
        text("Music:", 700, 520);
        fill(0);
        if (musicPlay == true)
            text("ON", 700, 555);
        else
            text("OFF", 700, 555);
        fill(255);
        textFont(smallFont);
        if (info3D == true)      //gives more information on the 3d game
        {                
            rect(196, 360, 220, 40, 10);
            fill(0);
            text("Venture through the universe, dodging asteroids!", 196, 360, 220, 40);
        }
        else if (info2D == true) //gives more information on the 2d game
        {
            rect(596, 360, 220, 40, 10);
            fill(0);
            text("Travel through an asteroid cavern and avoid making any contact with it!", 596, 360, 220, 40);
        }

        textFont(font);        
        fill(menu3D);
        rect(196, 280, 230, 115, 10);//left
        fill(menu2D);
        rect(596, 280, 230, 115, 10);//right
        fill(menuHighscore);
        rect(395, 425, 180, 50, 10);//highscore
        fill(menuInstructions);
        rect(395, 525, 180, 50, 10);
        fill(0);
        rect(width/2, height/12, 800, 40);  
        fill(255); 
        text("Interstellar: Into The Void", moveTitle, height/10);//title
        text("3D Adventure!", 200, 200);
        text("2D Adventure!", 590, 200);
        fill(255, 0, 0);
        text("GO!", 200, 285);//3d choice
        text("GO!", 590, 285);//2d choice        
        fill(0, 25, 240);
        text("High Score", width/2 - 6, height/2 + 135);
        text("Controls", width/2 - 7, height/2 + 235);
        moveTitle += increments;//moves the title

        if (moveTitle > 575 || moveTitle < 220)
            increments = -increments;
    }
}

//displays when the player dies and prompts them to input their name
void highScoreInput()
{
    fill(234, 103, 2);
    rectMode(CENTER);    
    rect(400, 300, 500, 300);
    fill(255);
    rect(400, 350, 500, 50);
    fill(ok);
    rect(400, 410, 100, 40, 10);
    fill(0);
    textAlign(CENTER);

    textFont(medFont);
    text("Please Input Your Name: ", width/2, height/2);    
    text("Your Score: " + score /60, width/2, height/2-100);   
    text("OK", width/2, 415); 

    //the following displays the name entered as well as a blinking cursor so the player knows they can still type
    textAlign(CORNER);
    text(name + (frameCount/10 % 4 == 0 ? "|" : ""), width/2 - 40, height/2 + 55);
}

//this method is passed the name and score variables so that the score can be checked to see if any records have been broken for the 3d game mode
void checkHighscore3D(String person, int theScore)
{
    if (theScore >= playerScore[2] && theScore < playerScore[1])
    {
        playerScore[2] =  theScore;
        player[2] =  person;
    }
    else if (theScore >= playerScore[1] && theScore < playerScore[0])
    {
        playerScore[2] = playerScore[1];
        player[2] = player[1];
        playerScore[1] =  theScore;
        player[1] =  person;
    }
    else if (theScore >= playerScore[0])
    {
        playerScore[2] = playerScore[1];
        player[2] = player[1];
        playerScore[1] = playerScore[0];
        player[1] = player[0];
        playerScore[0] =  theScore;
        player[0] =  person;
    }
}

//this method is passed the name and score variables so that the score can be checked to see if any records have been broken for the 2d game mode
void checkHighscore2D(String player, int playerScore)
{
    if (playerScore >= playerScore2D[2] && playerScore < playerScore2D[1])
    {
        playerScore2D[2] =  playerScore;
        player2D[2] =  player;
    }
    else if (playerScore >= playerScore2D[1] && playerScore < playerScore2D[0])
    {
        playerScore2D[2] = playerScore2D[1];
        player2D[2] = player2D[1];
        playerScore2D[1] =  playerScore;
        player2D[1] =  player;
    }
    else if (playerScore >= playerScore2D[0])
    {
        playerScore2D[2] = playerScore2D[1];
        player2D[2] = player2D[1];
        playerScore2D[1] = playerScore2D[0];
        player2D[1] = player2D[0];
        playerScore2D[0] =  playerScore;
        player2D[0] =  player;
    }
}

//when the player presses the button, the program will display all the highscores that have been set 
void displayHighScore()
{
    textAlign(CENTER);
    menuTemplate(); 
    fill(49, 140, 229, 200);   
    rect(width/2, 220, 650, 200);
    fill(225, 101, 23, 200);
    rect(width/2, 420, 650, 200);
    textFont(font);    
    fill(0);
    text("3D Adventure", width/2, 150);
    text("2D Adventure", width/2, 350);
    fill(255);
    text("HIGH SCORES", width/2, 60);    
    textAlign(LEFT);
    //for 3d
    text("1.    " + player[0], width/2 - 300, 200);
    text(playerScore[0], width/2 + 200, 200);
    text("2.    " + player[1], width/2 - 300, 250);
    text(playerScore[1], width/2 + 200, 250);
    text("3.    " + player[2], width/2 - 300, 300);
    text(playerScore[2], width/2 + 200, 300);

    //for 2d
    text("1.    " + player2D[0], width/2 - 300, 400);
    text(playerScore2D[0], width/2 + 200, 400);
    text("2.    " + player2D[1], width/2 - 300, 450);
    text(playerScore2D[1], width/2 + 200, 450);
    text("3.    " + player2D[2], width/2 - 300, 500);
    text(playerScore2D[2], width/2 + 200, 500);

    //for creating a file
    saveStrings("player3D.txt", player);        //savesthe 3d player names
    saveStrings("player2D.txt", player2D);      //saves the 2d player names
    String p3Scores[], p2Scores[];
    p3Scores = new String[3];
    p2Scores = new String[3];

    for (byte i = 0; i < playerScore.length; i++)
    {
        p3Scores[i] = String.valueOf(playerScore[i]);   //turns the scores into strings so that it is easier to save
    }
    for (byte i = 0; i < playerScore2D.length; i++)
    {
        p2Scores[i] = String.valueOf(playerScore2D[i]); //turns the scores into strings so that it is easier to save
    }
    saveStrings("score3D.txt", p3Scores);        //saves the 3d scores
    saveStrings("score2D.txt", p2Scores);        //saves the 2d scores
}

//displays the controls and instructions to play the game
void displayInstructions()
{
    menuTemplate();        //a menu template that is used amongst the instructions and the highscores
    fill(255);
    textFont(font);
    text("CONTROLS", width/2, 60);    
    rectMode(CORNER);
    fill(49, 140, 229, 200);
    rect(30, 110, 350, 400);
    fill(225, 101, 23, 200);
    rect(420, 110, 350, 400);
    stroke(0);
    fill(255);
    //for 3d adventure
    rect(180, 200, 50, 50);
    rect(180, 250, 50, 50);
    rect(230, 250, 50, 50);
    rect(130, 250, 50, 50);
    line(205, 235, 205, 215);//up
    line(200, 225, 205, 215);
    line(210, 225, 205, 215);
    line(205, 285, 205, 265);//down
    line(205, 285, 200, 275);
    line(205, 285, 210, 275);
    line(245, 275, 265, 275);//right
    line(255, 270, 265, 275);
    line(255, 280, 265, 275);
    line(145, 275, 165, 275);//left
    line(145, 275, 155, 280);
    line(145, 275, 155, 270);
    rect(105, 350, 200, 40);
    rect(495, 350, 200, 40);
    line(105, 460, 120, 460);

    //for 2d adventure
    rect(570, 200, 50, 50);
    line(595, 235, 595, 215);//up
    line(590, 225, 595, 215);
    line(600, 225, 595, 215);
    line(495, 460, 510, 460);
    pushMatrix();
    lights();
    noStroke();
    fill(234, 234, 0); 
    translate(75, 460, 0);   
    sphere(25);
    popMatrix();
    fill(0, 255, 0);
    ellipse(465, 460, 50, 50);
    fill(0);
    text("3D Adventure", 200, 140);
    text("2D Adventure", 600, 140);
    textFont(medFont);
    text("Space", 200, 375);
    text("Space", 590, 375);
    fill(255);
    text("Move Up", 205, 190);
    text("Move Right", 330, 275);
    text("Move Left", 80, 275);
    text("Move Down", 205, 320);
    text("Shoot (destroys asteroids!)", 205, 410);
    text("Ammo Pick-up", 195, 465);
    text("Hold to Ascend", 595, 190);
    text("Shoot (shortens stalactites/stalagmites)", 595, 410);
    text("Ammo Pick-up", 585, 465);
    textFont(smallFont);
    text("NOTE: The ship's max carrying capacity is 30 bullets", 600, 500);
    text("NOTE: The ship's max carrying capacity is 30 bullets", 210, 500);
    rectMode(CENTER);
}

//used amongst the instructions and the highscores as a background for aesthetics
void menuTemplate()
{    
    byte rSmall = 50;
    byte rMedium = 70;
    byte rLarge = 90;
    fill(255, 144, 39);
    rect(400, 50, 300, 60, 10);
    fill(202, 232, 14);
    //right side
    ellipse( 600, 50, rLarge, rLarge);
    ellipse( 690, 50, rMedium, rMedium);
    ellipse(760, 50, rSmall, rSmall);
    //left side
    ellipse( 200, 50, rLarge, rLarge);
    ellipse( 110, 50, rMedium, rMedium);
    ellipse(40, 50, rSmall, rSmall);
    //back Button
    fill(back);
    rect(400, 560, 100, 40, 10);
    textFont(font);
    fill(255);
    text("BACK", width/2, height-30);
}

//this method colours up the buttons when the cursor is on top of them
void mouseMoved()
{    
    if (run3D == false && run2D == false)
    {
        if (stage == "")//if the player is on the main screen
        {
            if (mouseX > 80 && mouseX < 310 && mouseY > 220 && mouseY < 335)                //if the player moves over the 3d game mode
            {
                menu3D = color(209, 255, 95);
                info3D = true;
            }

            else if (mouseX > 481 && mouseX < 711 && mouseY > 222.5 && mouseY < 337.5)      //if the player  moves over 2d game mode
            {                
                menu2D = color(209, 255, 95);
                info2D = true;
            }

            else if (mouseX > 305 && mouseX < 487 && mouseY > 400 && mouseY < 450)          //if the player  moves over highscore
                menuHighscore = color(242, 113, 7);


            else if (mouseX > 305 && mouseX < 487 && mouseY > 500 && mouseY < 550)          //if the player  moves over instructions
                menuInstructions = color(242, 113, 7);

            else                                                                            //this is to keep the buttons a specific colour when not hovered over 
            {
                menu3D = color(250, 194, 73);
                menu2D = color(250, 194, 73); 
                menuInstructions =color(250, 194, 73); 
                menuHighscore = color(250, 194, 73);
                info3D = false;
                info2D = false;
            }
        }

        //the colour changes when the cursor is on top of the back and ok buttons
        else if (stage == "show highscore" || stage == "instructions")
        {
            if (mouseX > 350 && mouseX < 450 && mouseY > 540 && mouseY < 580)
                back = color(240, 12, 229);

            else 
                back = color(255, 121, 249);
        }
        else if (stage == "score input")
        {
            if (mouseX > 350 && mouseX < 450 && mouseY > 390 && mouseY < 430) 
                ok = color(10, 98, 237);

            else 
                ok = color(2, 234, 221);
        }
    }
}
void mousePressed()
{
    if (run3D == false && run2D == false)
    {
        if (stage == "")
        {
            if (mouseX > 80 && mouseX < 310 && mouseY > 220 && mouseY < 335)                //if the player presses 3d game mode and the game activates
            {
                //backgroundMusic.pause();
                threeDSetup();                                                              //initializes the 3d variables needed
                run3D = true;                
                menu3D = color(250, 194, 73);
                info3D = false;
            }

            else if (mouseX > 481 && mouseX < 711 && mouseY > 222.5 && mouseY < 337.5)      //if the player presses 2d game mode and the game activates
            {     
                //backgroundMusic.pause();
                twoDSetup();                                                                //initializes the 2d variables needed
                run2D = true;
                menu2D = color(250, 194, 73);
                info2D = false;
            }

            else if (mouseX > 305 && mouseX < 487 && mouseY > 400 && mouseY < 450)           //if the player presses the highscore
            {
                stage = "show highscore";
                menuHighscore = color(250, 194, 73);
            }

            else if (mouseX > 305 && mouseX < 487 && mouseY > 500 && mouseY < 550)           //if the player presses the instructions
            {
                stage = "instructions";
                menuInstructions =color(250, 194, 73);
            }
            else if (mouseX >670 && mouseX < 730 && mouseY > 530 && mouseY < 570)
            {
                if (musicPlay == true)
                {
                    backgroundMusic.pause();
                    musicPlay = false; 
                    music = color(255, 0, 0);
                }
                else
                {
                    backgroundMusic.loop();
                    musicPlay = true;
                    music = color(38, 255, 28);
                }
            }
        }

        else if (stage == "show highscore" || stage == "instructions")
        {

            if (mouseX > 350 && mouseX < 450 && mouseY > 540 && mouseY < 580)                //if the player presses the back button when they are on the instruction or highscore screen
            {
                stage = "";
                back = color(255, 121, 249);
            }
        }
        else if (stage == "score input")
        {
            if (mouseX > 350 && mouseX < 450 && mouseY > 390 && mouseY < 430)                //if the player presses the ok button to agree with the name entry for the scores
            {            
                if (musicPlay == true)
                {
                    backgroundMusic = minim.loadFile("space.mp3");
                    //backgroundMusic.loop();
                }
                stage = "show highscore";
                if (dependingHighscore == "3D")                                              //the score gets thrown into the 3d highscore check if the player played the 3d game mode
                    checkHighscore3D(name, score/60);


                else if (dependingHighscore == "2D")                                         //the score gets thrown into the 3d highscore check if the player played the 3d game mode
                    checkHighscore2D(name, score/60);

                displayHighScore();                                                          //displays the highscores
                ok = color(2, 234, 221);
            }
        }
    }
}

//this method is to draw the background for the 3d game mode
void starBackground()
{
    for (int i = 0; i < stars.length;i++)
    {
        stars[i].drawStar();
    }
    pushMatrix();      
    fill(250, 184, 0);    //the yellow star  
    sphereDetail(20);
    translate(2000, -600, -3000);
    sphere(1000);
    fill(234, 103, 2);
    translate(-3000, 1600, 200);
    sphere(500);         //second star
    popMatrix();
}

//this method draws the 2d stars in the 2d game mode
void drawGalaxy()
{
    for (int i = 0; i < satellites.length; i++)
    {
        satellites[i].move();        //moves the stars
        satellites[i].collision();   //checks for collision to move them back to the other side of the screen
        satellites[i].draw();        //draws the stars
    }
}

//draws the two boxes that make up the plane
void drawPlane()
{
    fill(47, 99, 247);
    box(50, 50, 100);    //body of plane
    fill(192, 192, 192);
    box(200, 15, 10);    //wings of plane
}

//this method is for handling the ammo pickup in the 3d game mode
void moveGun()
{
    zGun += 8;
    if (zGun  > z -50  && zGun < 100)
    {               
        gunDistance = dist(x, y, z, xGun, yGun, abs(zGun));

        if (gunDistance < 60)
        {
            powerup.play();
            powerup = minim.loadFile("pickup.mp3");
            ammoLeft = 30 - ammunition;            //checks to see how much ammo can fit on the ship and then reacts accordingly
            positionOnce = true;                   //moves the ammo pickup far in the galaxy

            if (ammoLeft <= 30 && ammoLeft >= 11)  //if there is enough to pick up ten bullets
            {
                ammunition += 10;                  //gives the player ammo
            }
            else if (ammoLeft < 11)                //if the player has to pickup less than 10 bullets
            {
                ammunition += ammoLeft;            //gives the player the remaining ammo
            }
        }
        ammoLeft = 0;                              //resets the ammo
    }

    else if (zGun > 300)                           //runs only if the player did not pickup the ammo
    {
        positionOnce = true;
    }
}

//this method handles the progression for the 3d game mode
void gameProgression()
{
    moveGun();                            //moves the ammo pickup
    if (positionOnce == true)             //resets the ammo position
    {
        xGun = random(150, 800);
        yGun = random(150, 800);
        zGun = -7000;
        positionOnce = false;
    }

    //draws the power up
    fill(219, 253, 0);
    pushMatrix();
    translate(xGun, yGun, zGun);
    sphere(30);
    popMatrix();
}

void displayBullets()
{
    for (byte i = 0; i < bullets.length;i++)    
    {
        if (bullets[i].fired == true)            //runs only if the bullet is fired
        {
            bullets[i].moveBullets();            //bullet moves
            bullets[i].bulletCollision();        //checks for collision with other objects      
            bullets[i].drawBullets();            //draws the bullets
        }
    }
}

void display2DBullets()
{
    for (byte i = 0; i < shoot.length;i++)
    {
        if (shoot[i].shot == true)
        {        
            shoot[i].move();                    //moves the 2d bullets when shot
        }
    }
    for (byte i = 0; i < shoot.length;i++)
    {
        if (shoot[i].shot == true)
        {  
            shoot[i].collision();               //checks for collision with the environment when shot
        }
    }

    for (byte i = 0; i < shoot.length;i++)
    {
        if (shoot[i].shot == true)
        {  
            shoot[i].draw();                    //draws the bullets
        }
    }
}

//used to handle making the game harder as time passes on the 2d game mode
void progress()
{   
    if (score > 300)            //when the time permits, the ammo gets drawn
    {          
        pickup.move();          //moves the ammo pickup
        pickup.collision();     //collides the ammo pickup   
        pickup.draw();          //draws the ammo pickup
    }
    if (score > 800)            //when the time permits, the stalactite gets drawn
    {           
        wall.move();            //stalactite moves
        wall.collision();       //stalactite checks when to move back to the front
        wall.draw();            //draws the stalactite
    }
    if (score > 3000)           //when the time permits, the stalagmite gets drawn
    {
        bottom.move();          //stalagmite moves
        bottom.collision();     //stalagmite checks when to move back to the front
        bottom.draw();          //draws the stalagmite
    }    
    if (score % 1000 == 0)      //the following if block increases the speed at which everything moves as time passes
    {
        traverse += 1;
    }
}

//deals with moving the obstacles
void drawObstacles()
{
    if (time % 20 == 0)
    {
        if (obsAdvance < 300)
        {
            obsAdvance += 0.1;            //adds more asteroids to the map
        }
        speed += 0.06;                    //increases the speed at which the asteroids move by
    }

    for (byte i = 0; i < obsAdvance; i++)
    {        
        pushMatrix();
        dangerObstacles[i].move();                    //moves the asteroids

        if (dangerObstacles[i].visible == true)
        {            
            dangerObstacles[i].collision();           //checks for collision only if they are not shot
            dangerObstacles[i].drawObstacles();       //draws them only if they are not shot
        }
        popMatrix();
    }
}

void movePlane()
{    
    translate(x, y, z);//updates the position of the plane

    //this if block rotates the plane for it to look realistic
    if (bankRight == true)                //when the player wants to move right
    {
        x+=planeSpeed;
        rotateZ(10);
    }
    else if (bankLeft == true)            //when the player wants to move left
    {        
        x-=planeSpeed;
        rotateZ(-10);
    }
    if (bankUp == true)                   //when the player wants to move up
    {
        y-=planeSpeed;
        rotateX(-0.5);
    }
    else if (bankDown == true)            //when the player wants to move down
    {
        y+=planeSpeed;
        rotateX(0.5);
    }
}
//this moves the plane from the keystrokes. not in seperate key pressed method to make it look smoother
void keyPressed()
{    
    if (key == CODED)
    {
        if (keyCode == RIGHT)            //when the right arrow is pressed
            bankRight = true;

        else if (keyCode == LEFT)        //when the left arrow is pressed
            bankLeft = true;

        else if (keyCode == UP)          //when the up arrow is pressed
            bankUp = true;

        else if (keyCode == DOWN)        //when the down arrow is pressed
            bankDown = true;
    }
}

void keyReleased()
{
    //these if blocks just keep the plane from continuously moving if the player doesn't want to
    if (key == CODED)
    {
        if (keyCode == RIGHT)        
            bankRight = false;

        else if (keyCode == LEFT)
            bankLeft = false;

        else if (keyCode == UP)
            bankUp = false;

        else if (keyCode == DOWN)
            bankDown = false;
    }

    else
    {
        if (run2D == true || run3D == true)
        {
            if (key == ' ')                  //used to shoot bullets
            {
                if (ammunition > 0)          //only works if the player has ammunition
                {     
                    
                    if (run3D == true)       //this block sets the original position of the bullet in the 3d game      
                    {   
                        fire.play();                   
                        bullets[ammunition].xBull = x;
                        bullets[ammunition].yBull = y;
                        bullets[ammunition].zBull = z;
                        bullets[ammunition].fired = true;
                        ammunition--;
                    }
                    else if (run2D == true)  //this block sets the original position of the bullet in the 2d game 
                    {
                        if (timeToStart >= 3)//you can only shoot after the countdown
                        {   
                            fire.play();                      
                            shoot[ammunition].x2Bull = x;
                            shoot[ammunition].y2Bull = y;                    
                            shoot[ammunition].shot = true;
                            ammunition--;
                        }
                    }
                    fire = minim.loadFile("newFire.mp3");
                }
            }
        }
        else if (run3D == false && run2D == false && stage == "score input")
        {

            if (key == BACKSPACE)               //used for name input
            {
                if (name.length()>0)
                {
                    name = name.substring(0, name.length()-1);
                }
            }

            //this if block sends information to check the score depending on which game mode the player is
            else if (key == ENTER)
            {
                if (musicPlay == true)
                {
                    backgroundMusic = minim.loadFile("space.mp3");
                    //backgroundMusic.loop();
                }
                stage = "show highscore";
                if (dependingHighscore == "3D")
                {
                    checkHighscore3D(name, score/60);
                }
                else if (dependingHighscore == "2D")
                {
                    checkHighscore2D(name, score/60);
                }
                displayHighScore();
            }
            else if (name.length() < 15)       //used for the player to enter their name
            {
                name += key;
            }
        }
    }
}


//this method makes sure the player cannot leave the playing field.
void controlBounds()
{    
    //far left
    if (x < 0)
        x = 0;

    //far right
    else if (x > 800)
        x = 800;

    //far down
    if (y > 800)
        y = 800;

    //far up
    else if (y < 25)
        y = 25;
}

//displays information to the player for the 3d game mode
void HUD()
{
    fill(bar);
    rectMode(CENTER);

    //displays health bar
    pushMatrix();
    translate(x + 200, y + 10, z + 51);
    rect(-200, 50, recWidth, 30, 50);    
    popMatrix();   

    textFont(medFont);
    fill(255);
    //displays exact health left
    pushMatrix();
    text( health, x, y + 65, z + 52 );
    popMatrix();

    drawScoreKeeper(); // draws the current score

    if (ammunition > 0)
        displayAmmo();
}

//used to display info in the 2d game mode
void HUD2D()
{
    textFont(medFont);
    fill(255);
    text("Score: " + score/60, 700, 50);    
    text ("Ammo: " + ammunition, width/2, 50);
    fill(bar);
    text("Health: " + health, 100, 50);
    fill(150);
    if (score/60 > playerScore2D[0])
        text("Highscore: " + score/60, 700, 75);     //only if the score surpasses the highscore

    else
        text("Highscore: " + playerScore2D[0], 700, 75);
}
//displays the current ammunition for 3d game
void displayAmmo()
{
    pushMatrix();
    textFont(smallFont);
    text("Ammo: " + ammunition, x, y-45, z+52 );
    popMatrix();
}

//displays the current score
void drawScoreKeeper()
{
    textFont(huge);
    fill(255);
    text ("Current Score: " + score/60, 1700, 600, -2700);
    fill(170);
    if (score/60 > playerScore[0])
        text("Highscore: " + score/60, 1700, 900, -2700);        //only if the score surpasses the highscore

    else
        text("Highscore: " + playerScore[0], 1700, 900, -2700);
}
//keeps track of the current health status of the player
void determineColourOfHealthBar()
{
     hit.play();

    if (health > 75 && health <= 100)
        bar = color( 58, 255, 7);

    else if (health > 50 && health <=75)
        bar = color(170, 245, 0);

    else if (health > 25)
        bar = color( 245, 209, 0);

    else if (health > 0)
        bar = color( 245, 20, 0);

    else if (health <=0)
    {
        if (run3D == true) //runs only if the player dies in the 3d game
        {
            run3D = false;
            dependingHighscore = "3D";
            camera(400, 300, 520, 400, 300, 0, 0, 1, 0);//to keep it level
        }
        else if (run2D == true) //runs only if the player dies in the 2d game
        {
            run2D = false;            
            dependingHighscore = "2D";
        }
        if (musicPlay == false)
            backgroundMusic = minim.loadFile("space.mp3");   

        stage = "score input";
        name = "";
    }
    hit = minim.loadFile("newHit.mp3");
}

//CLASSES***********************************************************************************************************

//the obstacles class
class Obstacles
{
    float positionX, positionY, positionZ;     //holds the position of the asteroid
    float distance;                            //holds the distance from the player
    boolean visible;
    float theta, spin;                         //used for spinning the asteroid
    float vibrant;                             //used to fade in

    //sets the info of the asteroid 
    public Obstacles()
    {        
        speed = 10;
        randomPosition();                       //sets a random position for the asteroid
        visible = true;
        theta = 0;
        vibrant = 0;
        spin = random(-0.06, 0.06);
    }

    //sets a random position
    void randomPosition()
    {
        vibrant = 0;
        positionX = random(-200, 1000);
        positionY = random(-200, 1000);
        positionZ = random(-2500, -1600);

        if (visible == false)                   //resets the visibility
            visible = true;
    }

    //draws and spins the asteroids
    void drawObstacles()
    {
        fill(180, 180, 180, vibrant);               
        translate(positionX, positionY, positionZ);
        rotateY(theta);
        rotateZ(theta);
        rotateX(theta);
        sphereDetail(6);        //makes the asteroids look jagged
        sphere(50);             //draws the asteroids
        theta+= spin;
    }

    void move()
    {
        positionZ += speed;            //moves the obstacles
        if (positionZ >= 400)          //will give the asteroids a new place once they reached the suggested distance
            randomPosition();

        if (vibrant < 255)             //this makes the asteroids fade in
            vibrant += 20;
    }

    void collision()
    {
        if (visible == true)
        {
            if (positionZ > z+51)
            {                
                distance = dist(x, y, z, positionX, positionY, abs(positionZ));        //calculates the distance between the asteroids and the plane
                if (distance <= 90)
                {                    
                    health -= 12;                                                      //detractes from the players health when hit
                    recWidth -= 18;                                                    //makes the health bar smaller
                    shake = true;                                                      //will shake the screen
                }
            }
        }
    }
}

//star class meant to populate the background
class Star
{
    //star attributes
    float xCoor, yCoor, zCoor;
    float sWidth;
    color sClr;
    public Star()
    {
        //this places them far back so they look like stars
        xCoor = random(-2000, 6000);
        yCoor = random(-4000, 5000);
        zCoor = -3000;
        sWidth = random(20, 40);
        if (sWidth > 30)
            sClr = color(245, 210, 3);

        else
            sClr = color(146, 60, 240);
    }

    void drawStar() // draws the star and pushes it back to the edge of the map
    {        
        pushMatrix();        
        translate(xCoor, xCoor, zCoor);
        fill(sClr);
        ellipse(xCoor, yCoor, sWidth, sWidth);        
        popMatrix();
    }
}

//bullet class
class Bullet
{
    float xBull, yBull, zBull;  
    float distanceBetweenBulletandAsteroid;
    boolean fired;   

    void drawBullets()
    {
        fill(255, 0, 0);
        pushMatrix();
        translate(xBull, yBull, zBull);
        sphere(15);
        popMatrix();
    }

    void moveBullets()
    {
        zBull -= 50;
    }

    void bulletCollision()
    {
        for (int i = 0; i < dangerObstacles.length; i++)
        {
            if (dangerObstacles[i].visible == true)
            {
                if (dangerObstacles[i].positionZ > -1600)
                {                    
                    distanceBetweenBulletandAsteroid = dist(xBull, yBull, abs(zBull), dangerObstacles[i].positionX, dangerObstacles[i].positionY, abs(dangerObstacles[i].positionZ)); //calculates the distance between the bullet and the asteroid

                    if (distanceBetweenBulletandAsteroid <= 65)
                    {     
                        explosion();                                    //makes an explosion when the bullet hits asteroid
                        dangerObstacles[i].visible = false;                        
                        fired = false;
                    }
                }
            }

            if (zBull < -3000)
                fired = false;
        }
    }

    void explosion() // simulates an explosion
    {
        explodeSound.play();
        explodeSound = minim.loadFile("newExplosion.mp3");
        fill(240, 54, 17);               
        translate(xBull, yBull, zBull);
        sphere(150);
    }
}



//galaxy class used for asthetics in the main menu
class Gravity
{
    float xP, yP, pRadius, glow;
    float colourChooser; 
    byte xPSpeed;
    color starColour;
    public Gravity()
    {
        xP = random(width); 
        yP = random(height); 
        xPSpeed = byte(random(-5, -1));
        pRadius = abs(xPSpeed * 2);
        glow = 3* pRadius;
        colourChooser = random(1);
        if (colourChooser >= 0.5)
            starColour = color(255, 214, 3);

        else if (colourChooser <= 0.5)
            starColour = color(146, 60, 240);
    }

    void move()
    {
        xP+=xPSpeed;
    }


    void collision()
    {
        if (xP < 0)
            xP = width; //moves the stars back to the front of the map
    }
    void draw()
    {               
        fill(starColour);
        ellipse(xP, yP, pRadius, pRadius); //draws the star
        fill(starColour, 150);
        ellipse(xP, yP, glow, glow);       //draws the glow around the star
    }
}

class Ship
{
    float gravity = 0.3; 

    void move()
    {
        if (bankUp == true)                //moves the plane up with an acceleration
        {
            if (planeSpeed > -5)
                planeSpeed -= gravity;
        }

        else                               //moves the plane down with gravity
        {            
            if (planeSpeed < 8)
                planeSpeed += gravity;
        }
        y += planeSpeed;                   //moves the plane
    }

    void collision()
    {
        if (y < 20)
            y = 20;

        else if (y > 580)//to keep the plane from getting off the form
            y = 580;

        for (byte i = 0; i < cave.length; i++)
        {
            if ( cave[i].xPos + 13 > 75 && cave[i].xPos - 13 < 161) // isolates only the cave around the ship
            {
                if (y - 15 < cave[i].yPos - 150 )                   //used to detect the top of the cave
                {                    
                    health -= 24;                                   //the players health diminishes
                    determineColourOfHealthBar();                   //figures out what the players current health is
                    y = cave[i].yPos - 130;
                    planeSpeed = -planeSpeed;                       //makes the player bounce back so that they do not instantly die
                }
                else if (y + 10 > cave[i].yPos +150)                //used to detect the bottom of the cave
                {                    
                    health -= 24;                                   //the players health diminishes
                    determineColourOfHealthBar();                   //figures out what the players current health is
                    y = cave[i].yPos + 135;
                    planeSpeed = -planeSpeed;                       //makes the player bounce back so that they do not instantly die
                }
            }
        }
        if  (x + 30 >= pickup.xA - 10  && x - 55 <= pickup.xA + 10 && y - 15 <= pickup.yA + 10 && y + 10 >= pickup.yA - 10 ) // determines whether the player picked up ammo
        {
            powerup.play();
            powerup = minim.loadFile("pickup.mp3");
            pickup.xA = -20;                            //moves the ammo off screen so it can keep moving back
            ammoLeft = 30 - ammunition;                 //checks how much ammo you can still pick up
            if (ammoLeft <= 30 && ammoLeft >= 11)       //only runs when the player can pickup more than ten bullets
            {
                ammunition += 10;
            }
            else if (ammoLeft < 11)                     //only runs when the player can pick up less than ten bullets, it will give them the remainder
            {
                ammunition += ammoLeft;
            }

            ammoLeft = 0;                               //resets the difference to not interfere with later calculations
        }

        if (x + 30 > wall.sX - 13 && x - 55 < wall.sX + 13 && y - 10 < wall.sY + wall.sH)             //if the player runs into the stalactite
        {
            health = 0;                                                                               //instantly dies if player hits it
            determineColourOfHealthBar();                                                             //figures out what the players current health is
        }

        if (x + 30 > bottom.tX - 13 && x - 55 < bottom.tX + 13 && y + 10 > bottom.tY + bottom.tH)     //if the player runs into the stalactite
        {
            health = 0;                                                                               //instantly dies if player hits it
            determineColourOfHealthBar();                                                             //figures out what the players current health is
        }
    }

    //draws the plane
    void draw()
    {     
        translate(x, y);   
        fill(bar); 
        //body
        beginShape();
        vertex(-55, -10);
        vertex(0, -10);
        vertex(10, 0);
        vertex(30, 6);
        vertex(10, 15);
        vertex(-55, 15);    
        endShape();
        //wing
        fill(0);
        beginShape();
        vertex( -30, 0);
        vertex(-5, 0);
        vertex( -40, 10);
        endShape();
        //tail
        fill(0, 0, 255);
        beginShape();
        vertex(-55, -10);
        vertex(-55, -20);
        vertex(-45, -20);
        vertex(-30, -10);
        endShape();
        //flame
        fill(247, 122, 7);
        beginShape();
        vertex(-55, -10);
        curveVertex(-65, -20);
        curveVertex(-60, -5);
        curveVertex(-80, 0);
        curveVertex(-60, 10);
        curveVertex(-65, 20);
        vertex(-55, 15);
        endShape();
    }
}


class Bullet2D
{
    float x2Bull, y2Bull;
    boolean shot;        //holds the info regarding whether it was shot or not

        public Bullet2D()
    {
        shot = false;    //sets everything to not be shot so they can be used
    }
    void draw()
    {
        fill(255, 0, 0);
        ellipse(x2Bull, y2Bull, 10, 10);//draws the bullet
    }

    void move()
    {
        x2Bull += 10;                   //moves the bullet
    }

    void collision()
    {
        if (x2Bull > width)             //if the bullet extends past the screen, it can be used again
            shot = false;

        fill(255, 0, 0);
        for (byte i = 0; i < cave.length; i++)
        {
            if (x2Bull + 5 > cave[i].xPos - 12 && x2Bull - 5 < cave[i].xPos + 12)           //isolates the collision check to only where the bullet is (x value)
            {     
                if (y2Bull + 5 > cave[i].yPos + 150  || y2Bull - 5 < cave[i].yPos - 150)    //checks to see if the bullet hits the side of the cave
                {
                    explode();                                                              //if it does, it will blow up
                    break;                                                                  //will stop checking for additional collisions
                }
            }
        }


        if (x2Bull + 5 > wall.sX - 12 && x2Bull - 5 < wall.sX + 12 && y2Bull + 5 > wall.sY - 150 && y2Bull - 5 < wall.sY + wall.sH)            //checks to see if the bullet hits the stalactite
        {            
            explode();                                                                                                                         //if it does, it will explode and shorten the stalactite
            wall.sH -= 50;
        }
        if (x2Bull + 5 > bottom.tX - 13 && x2Bull - 5 < bottom.tX + 13 && y2Bull - 5 < bottom.tY + 151 && y2Bull + 5 > bottom.tY + bottom.tH)  //checks to see if the bullet hits the stalagmite
        {
            explode();                                                                                                                         //if it does, it will explode and shorten the stalagmite
            bottom.tH += 50;
        }
    }

    void explode() //simulates an explosion from the bullet
    {
        explodeSound.play();
        explodeSound = minim.loadFile("newExplosion.mp3");
        ellipse(x2Bull, y2Bull, 60, 60);
        shot = false;
    }
}

//class meant to draw the path for the 2d game mode
class Path
{
    float xPos, yPos;
    float colourChange;
    int changeBy;
    boolean redraw;     //used to know when it should be replaced

    public Path(float givenX, float givenY)        //constructor to initially place the path
    {
        yPos = givenY;
        xPos = givenX;        
        redraw = false;
        colourChange = 0;
        changeBy = 1;
    }

    void move()                                    //used to move the path
    {
        xPos -= traverse;
    }
    void collision()
    {
        if (xPos <= -15)
        {
            xPos = cave[caveLastSlot].xPos + 26;   //if the path block gets to the back of the screen, it is placed right behind the slot in front of it            
            yPos = 160 + 350 * noise(changeSR);    //creates a cool path that alternates in the y values just slightly to make it look like a wave
            changeSR += 0.08;           

            caveLastSlot ++;                       //keeps track of the last slot moved so the next can be placed behind it
            if (caveLastSlot == 32)
                caveLastSlot = 0;
            redraw = true;
        }
    }
    void draw()
    {    
        if (colourChange < 1)                       //the following if blocks is to just change the colour for aesthetics
            changeBy = -changeBy;

        else if (colourChange > 255)
            changeBy = -changeBy;

        colourChange += changeBy;
        colorMode(HSB);                             //change the colour mode because it is easier to change the colour in a more smoother transition
        fill(colourChange, 100, 100);        
        rect(xPos, yPos, 26, 300);                  //draws the path blocks
        colorMode(RGB);
    }
}

//this class is to take care of the ammo pick up in the 2d game mode
class Ammo
{
    float xA, yA; 

    void draw()
    { 
        fill(0, 255, 0);
        ellipse(xA, yA, 40, 40);
    }

    void place()
    {
        for (byte i = 0; i < cave.length; i++)
        {
            if (cave[i].xPos > width )//this is to keep the ammo from being spawned on the screen but rather just off so it looks smooth
            {
                xA = cave[i].xPos;
                yA = cave[i].yPos;
                break;
            }
        }
    }

    void move()
    {
        xA -= traverse;
    }
    void collision()
    {
        if (xA < -4500)  //to make the ammo spawn in intervals
            place();
    }
}

class Stalactite
{
    float sX, sY, sH; 

    public Stalactite()
    {        
        sH = 0;
    }
    void locate()
    {       
        for (byte i = 0; i < cave.length; i++)
        {
            if (cave[i].xPos > width) //this is to keep the stalactite from being spawned on the screen but rather just off so it looks smooth
            {
                sX = cave[i].xPos;
                sY = cave[i].yPos;
                break;
            }
        }
    }

    void draw()
    {
        fill(200);
        triangle(sX - 13, sY - 151, sX + 13, sY - 151, sX, sY + sH); //this is meant to be able to draw a stalactite on any slot of the cave class array the program sees fit
    }

    void move()
    {
        sX -= traverse;
    }
    void collision()
    {
        //the following if blocks is just to regulate how often the stalactite reappears as the game progresses
        if (score < 5000)
        {
            if (sX + 13 < -1000)
            {
                locate();
                sH = 0;
            }
        }
        else if (score >= 5000)//appears faster
        {
            if (sX + 13 < -500)
            {
                locate();
                sH = 0;
            }
        }
    }
}

class Stalagmite
{
    float tX, tY, tH; 

    public Stalagmite()
    {        
        tH = 0;
    }
    void locate()
    {                       
        for (byte i = 0; i < cave.length; i++)
        {
            if (cave[i].xPos > width) //this is to keep the stalagmite from being spawned on the screen but rather just off so it looks smooth
            {
                tX = cave[i].xPos;
                tY = cave[i].yPos;
                break;
            }
        }
    }

    void draw()
    {
        fill(200);
        triangle(tX - 13, tY + 151, tX + 13, tY + 151, tX, tY + tH); //this is meant to be able to draw a stalagmite on any slot of the cave class array the program sees fit
    }

    void move()
    {
        tX -= traverse;
    }
    void collision()
    {
        //the following iff blocks is just to regulate how often the stalactite reappears as the game progresses
        if (score < 5000)
        {
            if (tX + 13 < -1750)
            {           
                locate();
                tH = 0;
            }
        }
        else if (score >= 5000)//appears faster
        {
            if (tX + 13 < -1250)
            {           
                locate();
                tH = 0;
            }
        }
    }
}
