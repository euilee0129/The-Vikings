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
    private int verticalSpeed = 0;
    private int speed = 3;
    private int countJump;
    private boolean jumping = false;

    public void checkFall()
    {
        if(onGround() )
        {
            verticalSpeed = 0;
        }
        else 
        {
            fall();
        }
    }

    public void checkKeys()
    {
        if( Greenfoot.isKeyDown("up") )
        {
            doubleJump();
        }
        if( Greenfoot.isKeyDown("left") )
        {
            moveBack();
        }
        if( Greenfoot.isKeyDown("Right") )
        {
            moveForward();
        }
        if (Greenfoot.isKeyDown("l")) 
        {
            throwAxe();
        }
    }

    /**
     * Act - do whatever the Viking wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        checkFall();
        checkKeys();
        axeDelayCount++;
    }  
    private static final int AxeReloadTime = 30;
    private int axeDelayCount = 100;
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

    public void jump() 
    {
        verticalSpeed = -15;
        fall();
    }

    public void fall()
    {
        setLocation(getX(), getY() + verticalSpeed);
        verticalSpeed++;
    }

    public boolean onGround()
    {
        Actor under = getOneObjectAtOffset( 0, getImage().getHeight()/2, Ship.class );
        return under != null;
    }

    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
    }

    public void doubleJump()
    {
        if(countJump >= 2 && onGround() )
        {
            countJump = 0;
            jumping = false;
        }
        if(Greenfoot.isKeyDown("up") && jumping == false )
        {
            countJump++;
            jumping = true;
            jump();
        }
        if(jumping  == true && countJump == 1 )
        {
            jump();
            countJump++;
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
import java.util.ArrayList;
/**
 * Write a description of class Valkyrie here.
 * 
 * @author Samuel Lee and Marc Jung
 * @version (a version number or a date)
 */
public class Valkyrie extends Actor
{
    private final int HEART = 0;
    private ArrayList< Life > health = new ArrayList< Life >();
    public Valkyrie()
    {
        health.add( new Life() );
        health.add( new Life() );
        health.add( new Life() );
    }
    public void act()
    {
        removeLife();
    }
    public ArrayList <Life> getHealth()
    {
        return health;
    }
    public void gameOver()
    {
        if( health.size() == 0 )
        {
            Greenfoot.stop();
        }
    }
    public boolean hit()
    {
        if(this.isTouching(Axe.class) )
        {
            return true;
        }
        return false;
    }
     public void removeLife()
    {
        World world = getWorld();
        int x = 1010;
        if(isTouching(Axe.class) )
        {
            world.removeObject(getOneObjectAtOffset(x , 40, Life.class) );
            x += 50;
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

Valk:

import greenfoot.*;  // (World, Actor, GreenfootImage, Greenfoot and MouseInfo)
import java.util.ArrayList;
/**
 * Write a description of class Valkyrie here.
 * 
 * @author Samuel Lee and Marc Jung
 * @version (a version number or a date)
 */
public class Valkyrie extends Actor
{
     private int verticalSpeed = 0;
    private int speed = 3;
    private int countJump;
    private boolean jumping = false;

    public void checkFall()
    {
        if(onGround() )
        {
            verticalSpeed = 0;
        }
        else 
        {
            fall();
        }
    }

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
        if (Greenfoot.isKeyDown("space")) 
        {
            throwSword();
        }
    }

    /**
     * Act - do whatever the Viking wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        checkFall();
        checkKeys();
        swordDelayCount++;
    }  
    private static final int SwordReloadTime = 30;
    private int swordDelayCount = 100;
    /**
     * Throw axe everytime axeDelayCount reaches axeReloadTime
     */
    private void throwSword() 
    {
        if ( swordDelayCount >= SwordReloadTime) 
        {
            Sword sword = new Sword();
            getWorld().addObject( sword, getX(), getY() );
            sword.arch();
            swordDelayCount = 0;
        }
    }

    public void jump() 
    {
        verticalSpeed = -15;
        fall();
    }

    public void fall()
    {
        setLocation(getX(), getY() + verticalSpeed);
        verticalSpeed++;
    }

    public boolean onGround()
    {
        Actor under = getOneObjectAtOffset( 0, getImage().getHeight()/2, Ship.class );
        return under != null;
    }

    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
    }

    public void doubleJump()
    {
        if(countJump >= 2 && onGround() )
        {
            countJump = 0;
            jumping = false;
        }
        if(Greenfoot.isKeyDown("up") && jumping == false )
        {
            countJump++;
            jumping = true;
            jump();
        }
        if(jumping  == true && countJump == 1 )
        {
            jump();
            countJump++;
        }
    }
    // public void removeLife()
    // {
        // World world = getWorld();
        // int x = 1010;
        // if(isTouching(Axe.class) )
        // {
            // world.removeObject(getOneObjectAtOffset(x , 40, Life.class) );
            // x += 50;
        // }
    // }
}

-----------------------------


