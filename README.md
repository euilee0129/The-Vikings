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





