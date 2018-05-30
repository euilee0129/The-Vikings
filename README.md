# The-Vikings
Ap computer science final Greenfoot game


SoreBoarrd:

import greenfoot.*;  // (World, Actor, GreenfootImage, and Greenfoot)
import java.util.Calendar;
/**
 * The ScoreBoard is used to display results on the screen. It can display some
 * text and several numbers.
 * 
 * @author Marc Jung and Samuel Lee
 * @cred to M. Kolling
 * @version 1.0
 */
public class ScoreBoard extends Actor
{
    public static final float FONT_SIZE = 48.0f;//the size of the word 'Game Over"
    public static final int WIDTH = 350;// sets the width of the scoreBoard
    public static final int HEIGHT = 250;//gets the height of the scoreBoard
    /**
     * Create a score board for the final result.
     */
    public ScoreBoard(int score)
    {
        makeImage("GAME OVER", "Life: ", score );
    }

    /**
     * Make the score board image.
     */
    private void makeImage(String title, String prefix, int score)
    {
        GreenfootImage image = new GreenfootImage(WIDTH, HEIGHT);

        image.setColor(new Color(255,255,255, 128));
        image.fillOval(0, 0, WIDTH, HEIGHT);
        image.setColor(new Color(0, 0, 0, 128));
        image.fillOval(5, 5, WIDTH-10, HEIGHT-10);
        Font font = image.getFont();
        font = font.deriveFont(FONT_SIZE);
        image.setFont(font);
        image.setColor(Color.WHITE);
        image.drawString(title, 30, 100);
        image.drawString(prefix + score, 100, 200);
        setImage(image);
    }
}
------------------
Viking:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)

/**
 * The Vikings
 * 
 * @author Samuel Lee and Marc Jung 
 * @version 1.0
 * 
 * 
 */
public class Viking extends Actor
{
    private int verticalSpeed = 0; //acceleration when falling
    private int speed = 3; // control how fast the VikingShip moves
    private int countJump; //count the number of times the Viking jumps
    private boolean jumping = false; //Checks whether Viking jumps
    private static final int AxeReloadTime = 30;
    private int axeDelayCount = 100;
    
    
    /**
     * Act - do whatever the Viking wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        checkFall();
        checkKeys();
        death();
        axeDelayCount++;
    } 
    
    /**
     * Checks whether Viking is on VikingShip
     */
    public void checkFall()
    {
        if(onGround() ) //checks whether Viking is on ship
        {
            verticalSpeed = 0; //no acceleration
        }
        else 
        {
            fall(); //falls if not on VikingShip
        }
    }
    
    /**
     * Assigns certain keys to move Viking
     */
    public void checkKeys() 
    {
        if( Greenfoot.isKeyDown("up") ) //up allows it to jump
        {
            doubleJump();
        }
        if( Greenfoot.isKeyDown("left") ) //left allows it to move to the left
        {
            moveBack();
        }
        if( Greenfoot.isKeyDown("Right") ) //right allows it to move to the right
        {
            moveForward();
        }
        if (Greenfoot.isKeyDown("l")) //"l" allows it to move to throw weapon
        {
            throwAxe();
        }
    } 

    /**
     * Throw axe everytime axeDelayCount reaches axeReloadTime
     */
    private void throwAxe() 
    {
        if ( axeDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            axeDelayCount = 0;
        }
    }

    /**
     * Assigns vertical speed a negative integer to move 
     * upward when fall() is called
     */
    public void jump() 
    {
        verticalSpeed = -15; 
        fall();
    }
    
    /**
     * Moves Viking either upward or downward by vertical speed
     */
    public void fall()
    {
        setLocation(getX(), getY() + verticalSpeed); //moves Viking by verticalSpeed
        verticalSpeed++; //acceleration due to gravity
    }
    
    /**
     * Checks whether Viking is on Ship
     */
    public boolean onGround()
    {
        Actor under = getOneObjectAtOffset( 0, getImage().getHeight()/2, VikingShip.class );
        //under holds all instances of Vikings to be checked
        return under != null; //checks whether under is instantiated
    }
    
    /**
     * Moves Viking forward by a fixed speed
     */
    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    /**
     * Moves Viking backward by a fixed speed
     */
    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
    }

    /**
     * Limits the number of times Viking can jump
     */
    public void doubleJump()
    {
        if(countJump >= 2 && onGround() ) //checks whether Viking has jumped twice
        {
            countJump = 0; //reset countJump to 0
            jumping = false; //reset jumping to false
        }
        if(Greenfoot.isKeyDown("up") && jumping == false ) //1st jump
        {
            countJump++; //increment countJump by one
            jumping = true; //assigns jumping to be true
            jump(); //calls jump() method
        }
        if(jumping  == true && countJump == 1 ) //2nd jump
        {
            jump(); //calls jump() method
            countJump++; //increment countJump by one
        }
    }
    
    /**
     * Return a boolean value for conditional statement
     */
    public boolean hit()
    {
        if(this.isTouching(Sword.class) ) //checks whether Viking has been hit by a sword
        {
            return true;
        }
        return false;
    }
    
    private int vikingX = 40;
    /**
     * Ends game when life points are gone
     */
    public void death()
    {
        ValkyrieLifeEater lifeEater = new ValkyrieLifeEater(); //Destroy a life when
        World world0 = getWorld();                         // conditions are met 
        if(hit() )                                       
        {
            world0.addObject(lifeEater, vikingX, 40);
            //position lifeEater to eat life
            lifeEater.removeLife(); //removes a life when lifeEater touches a life
            vikingX += 40;
        }
        if(lifeEater.returnCount() == 3 ) //ends game when life points are gone
        {
            MyWorld world = (MyWorld) getWorld();
            world.gameOver(); //ends game
        }
    }
}

---------------
Axe:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
import java.util.ArrayList;
/**
 * Weapon for viking
 * 
 * @author Samuel Lee and Marc Jung
 * @version 1.0
 */
public class Axe extends Actor
{
    Viking player1 = new Viking();
    Valkyrie player2 = new Valkyrie();
    ArrayList<Life> p2= new ArrayList<Life>();
    private int countDamage = 0 ;
    /**
     * Axe constructor
     */
    public Axe()
    {
        Greenfoot.playSound( "Viking axe.wav" );
        Life life1 = new Life();
        Life life2 = new Life();
        Life life3 = new Life();
        p2.add(life1);
        p2.add(life2);
        p2.add(life3);
    }

    /**
     * Act - do whatever the Axe wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        arch();
    } 

    private int takeDamage()// for marc, does not work
    { 
        int count = 0;
        if(isTouching(Valkyrie.class) )
        {
            count++;
        }
        return count;
    }

    public void location()
    {
        setLocation(player1.getX(), player1.getY() );
    }

    public void damage()// marc does not in use
    {
        if(takeDamage() != 0)
        {
            while( countDamage > 0 )
            {
                World world = getWorld();
                world.removeObject(p2.get(countDamage) );
                countDamage--;

            }

        }
    }
    private boolean thrown = false;

    private double posx, posy, vx, vy, ax, ay, dt=0.1;
    /**
     * do this when the actor is added to the world
     */
    public void addedToWorld( World MyWorld )
    {
        posx = getX();
        posy = getY();

        ax= 0;
        ay= 10;

        vx= 50;
        vy=-70;

    }
    /**
     * parabola of axe
     */
    public void arch()
    {
        posx += vx*dt+0.5*ax*dt*dt;//pixels of object at X-axis
        posy += vy*dt+0.5*ay*dt*dt;//pixels of object at Y-axis

        vx+= ax*dt;//
        vy+= ay*dt;//

        setLocation((int)posx, (int)posy );//set object to posx and posy coordinates
        if( isAtEdge() == true || isTouching( Valkyrie.class) == true )
        {
            World world = getWorld();
            world.removeObject( this);
        }
    }
}

----------------

World:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
import java.util.ArrayList;
/**
 * Viking game's world
 * 
 * @author Samuel Lee and Marc Jung
 * @version 1.0
 */
public class MyWorld extends World
{
    /**
     * Constructor for objects of class MyWorld.
     * 
     */
    public MyWorld()
    {    
        // Create a new world with 1150 x 600 cells with a cell size of 1x1 pixels.
        super(1150, 600, 1);
        
        Life vikinglife = new Life(); //1st heart
        this.addObject( vikinglife, 40, 40 );//differences of 50
        
        Life vikinglife1 = new Life(); //2nd heart
        this.addObject( vikinglife1, 90, 40 );
        
        Life vikinglife2 = new Life(); //3rd heart
        this.addObject( vikinglife2, 140, 40 );

        
        Life valkyrielife = new Life(); //1st heart
        this.addObject( valkyrielife, 1010, 40 );//differences of 50
        
        Life valkyrielife1 = new Life(); //2nd heart
        this.addObject( valkyrielife1, 1060, 40 );
        
        Life valkyrielife2 = new Life(); //3rd heart
        this.addObject( valkyrielife2, 1110, 40 );
        
        Viking viking = new Viking();
        Valkyrie valk = new Valkyrie();
        Ship ship = new Ship();
        //Sword sword = new Sword();
        addObject( viking, 200, getHeight()-200 );
        addObject( valk, 300, getHeight()-200 );
        valk.act();
        addObject( ship, viking.getX(), viking.getY()+50);
        addObject( new Ocean(), getWidth()/2, 599 );
        
    }
    /**
     * This method is called when the game is over to display the final score.
     */
    public void gameOver() 
    {
        addObject(new ScoreBoard(-3), getWidth()/2, getHeight()/2);
    }
    
    }

----------------

Valkyrie:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
/**
 * Write a description of class Valkyrie here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class Valkyrie extends Actor
{ 
    private int verticalSpeed = 0; //acceleration when falling
    private int speed = 3; //controls how fast ValkyrieShip moves
    private int countJump; //count # of times Valkyrie jumps
    private boolean jumping = false; //checks whether Valkyrie has jumped
    private static final int AxeReloadTime = 30;
    private int axeDelayCount = 100;
    
    /**
     * Act - do whatever the Viking wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        checkFall();
        checkKeys();
        death();
        axeDelayCount++;
    }  

    /**
     * Assigns vertical speed a negative integer to move 
     * upward when fall() is called
     */
    public void jump() 
    {
        verticalSpeed = -15;
        fall();
    }

    /**
     * Moves Valkyrie either upward or downward by vertical speed
     */
    public void fall()
    {
        setLocation(getX(), getY() + verticalSpeed); //moves Valkyrie by verticalSpeed
        verticalSpeed++; //acceleration due to gravity
    }

    /**
     * Checks whether Valkyrie is on Ship
     */
    public boolean onGround()
    {
        Actor under = getOneObjectAtOffset( 0, getImage().getHeight()/2, ValkyrieShip.class );
        //under holds all instances of Valkyrie to be checked
        return under != null; //checks whether under is instantiated
    }

    /**
     * Checks whether Valkyrie is on ValkyrieShip
     */
    public void checkFall()
    {
        if(onGround() ) //checks whether Valkyrie is on ship
        {
            verticalSpeed = 0; //no acceleration
        }
        else 
        {
            fall(); //falls if not on ValkyrieShip
        }
    }

    /**
     * Moves Valkyrie forward by a fixed speed
     */
    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    /**
     * Moves Valkyrie backward by a fixed speed
     */
    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
    }

    /**
     * Assigns certain keys to move Valkyrie
     */
    public void checkKeys()
    {
        if( Greenfoot.isKeyDown("w") )
        {
            doubleJump();
        }
        if( Greenfoot.isKeyDown("a") )
        {
            moveBack();
        }
        if( Greenfoot.isKeyDown("d") )
        {
            moveForward();
        }
        if(Greenfoot.isKeyDown("space") )
        {
            throwAxe();
        }
    }

    /**
     * Limits the number of times Valkyrie can jump
     */
    public void doubleJump() 
    {
        if(countJump >= 2 && onGround() ) //checks whether Valkyrie has jumped twice
        {
            countJump = 0; //reset countJump to 0
            jumping = false; //reset jumping to false
        }
        if(Greenfoot.isKeyDown("up") && jumping == false ) //1st jump
        {
            countJump++; //increment countJump by one
            jumping = true; //assigns jumping to be true
            jump(); //calls jump() method
        }
        if(jumping  == true && countJump == 1 ) //2nd jump
        {
            jump(); //calls jump() method
            countJump++; //increment countJump by one
        }
    }
    
    /**
     * Throw axe everytime axeDelayCount reaches axeReloadTime
     */
    private void throwAxe() 
    {
        if ( axeDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            axeDelayCount = 0;
        }
    }
    
    /**
     * Return a boolean value for conditional statement
     */
    public boolean hit()
    {
        if(this.isTouching(Axe.class) ) //checks whether Valkyrie has been hit by a sword
        {
            return true;
        }
        return false;
    }


    private int valkyrieX = 40;
    /**
     * Ends game when life points are gone
     */
    public void death()
    {
        ValkyrieLifeEater lifeEater = new ValkyrieLifeEater(); //Destroy a life when
        World world0 = getWorld();                         // conditions are met 
        if(hit() )                                       
        {
            world0.addObject(lifeEater, valkyrieX, 40);
            //position lifeEater to eat life
            lifeEater.removeLife(); //removes a life when lifeEater touches a life
            valkyrieX += 40;
        }
        if(lifeEater.returnCount() == 3 ) //ends game when life points are gone
        {
            MyWorld world = (MyWorld) getWorld();
            world.gameOver(); //ends game
        }
    }

}
-------------

Life:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
import java.util.ArrayList;
/**
 * Life class to display players' lives.
 * 
 * @author Samuel Lee and Marc Jung 
 * @version 1.0
 */
public class Life extends Actor
{
    private final int HEART = 0;
    private ArrayList< Life > health = new ArrayList< Life >();
    public Life()
    {
        health.add( this );
        health.add(this );
        health.add( this );
    }
    World world = getWorld();
    /**
     * Act - do whatever the Life wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        addedToWorld( world );
    }
    
    public void addedToWorld( World world )
    {
        world = getWorld();
        //remove
        
    }
    
    
   }
-------------

Ocean:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
/**
 * Ocean class that ends the game if either player one or player two falls into the ocean and "drowns"
 * 
 * @author Samuel Lee and Marc Jung
 * @version 2.0
 */
public class Ocean extends Actor
{
    /**
     * Act - do whatever the Ocean wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {   
        drown();
    }    
    /**
     * Check if player has fallen into the ocean
     */
    public void drown()
    { 
        if(this.isTouching(null) )
        {
            Greenfoot.playSound( "dead sound.wav" );//plays a sound when the object is drowning
            this.removeTouching(null);//removes the object that this touching the ocean
            MyWorld world = (MyWorld) getWorld();//gets the world that the Ocean is in
            world.gameOver();//calls the gameOver method on the world, to end the game
        }
    }
}

----------------

VikingShip:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)

/**
 * Write a description of class VikingShip here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class VikingShip extends Actor
{
    private int speed = 3;
    
    public void act() 
    {
        move();
        playerOn();
    }    
    public void move()
    {
        World world = getWorld();
        if(isAtEdge() || this.getX() >= world.getWidth()/2)
        {
            speed = -speed;
        }
        setLocation(getX() + speed, getY() );
    }
    public void playerOn()
    {
        Actor player = getOneIntersectingObject(Viking.class);
        if(this.isTouching(Viking.class) && player != null)
        {
            player.setLocation(player.getX() + speed, player.getY() );
        }
    }   
}

--------------------------------------------------------------------------

ValkyrieShip:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)

/**
 * Write a description of class ValkyrieShip here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class ValkyrieShip extends Actor
{
    private int speed = 3;

    public void act() 
    {
        move();
        playerOn();
    }    
    public void move()
    {
        World world = getWorld();
        if(isAtEdge() || this.getX() <= world.getWidth()/2)
        {
                speed = -speed;
        }
        setLocation(getX() + speed, getY() );
    }
    public void playerOn()
    {
        Actor player = getOneIntersectingObject(Valkyrie.class);
        if(this.isTouching(Valkyrie.class) && player != null)
        {
            player.setLocation(player.getX() + speed, player.getY() );
        }
    }    
}

-------------------------------------------------------------------------

Viking LifeEater:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)

/**
 * Write a description of class VikingLifeEater here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class VikingLifeEater extends Actor
{

    private int count;

    public void act() 
    {
        removeLife();
    }    
    public void removeLife()
    {
        if( this.isTouching(Life.class) )
        {
            this.removeTouching(Life.class);
            count++;
        }
    }

    public int returnCount()
    {
        return count;
    }   
}

--------------------------------------------------------------

Valkyrie LifeEater:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)

/**
 * Write a description of class ValkyrieLifeEater here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class ValkyrieLifeEater extends Actor
{
    private int count;

    public void act() 
    {
        removeLife();
    } 
    
    public void removeLife()
    {
        if( this.isTouching(Life.class) )
        {
            this.removeTouching(Life.class);
            count++;
        }
    }

    public int returnCount()
    {
        return count;
    }  
}

Viking
    /**
     * Ends game when life points are gone
     */
    public void death()
    {
        VikingLifeEater eater = new VikingLifeEater(); //Destroy a life when
        World world0 = getWorld();                         // conditions are met 
        if(hit() )                                       
        {
            world0.addObject(new VikingLifeEater(), vikingX, 40); //position lifeEater to eat life
            eater.removeLife(); //removes a life when lifeEater touches a life
            vikingX += 50;
        }
        if(eater.returnCount() == 3 ) //ends game when life points are gone
        {
            MyWorld world = (MyWorld) getWorld();
            world.gameOver(); //ends game
        }
    }
    
    Valkyrie
    
    private int valkyrieX = 1010;
    /**
     * Ends game when life points are gone
     */
    public void death()
    {                                                        //Destroy a life when
        World world0 = getWorld(); 
        ValkyrieLifeEater eater = new ValkyrieLifeEater();                                                     // conditions are met 
        if(hit() )                                       
        {
            world0.addObject(new ValkyrieLifeEater(), valkyrieX, 40);
            //position lifeEater to eat life
            valkyrieX += 50;
        }
        if(eater.returnCount() == 3 ) //ends game when life points are gone
        {
            MyWorld world = (MyWorld) getWorld();
            world.gameOver(); //ends game
        }
    }
