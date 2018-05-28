# The-Vikings
Ap computer science final Greenfoot game

P.S. using the blame button above the edit button lets u see time stamps of who did what to "blame" anyone lol
and using branches u can request a pull request, it's actually pretty easy


later find pictures that fit, u might have to edit sizes and stuff, and add sound effect
We'll start commenting once we finish

    
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
        //addObject( sword, 1000, getHeight()-200 );
        //addObject( new Ship(), getObjectsAt( 200, getHeight()-200, Viking.class).getX(), getObjectsAt( 200, getHeight()-200, Viking.class).getY(), Ship.class );
        addObject( ship, viking.getX(), viking.getY()+50);
        addObject( new Ocean(), getWidth()/2, 599 );
        Block wall = new Block();
        addObject( wall, getWidth()/2, ship.getY() );
        ship.act(); ship.isMiddle();
        
    }
    public void act()
    {
        //removeLife();
    }
    
    }
    
    
    ------------------------------------------
    
    public class Block extends Actor
{
    public Block()
    {
        setRotation(-90);
    }
    /**
     * Act - do whatever the Block wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        setRotation(-90);
    }    
}
    
    ----------------------------------
    
   public class Ship extends Actor
{
    private int speed = 2;
    /**
     * Act - do whatever the Ship wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        move();
        playerOn();
    }    
    private int centX;
    private int centY;
    public void move()
    {
        if(isAtEdge() )
        {
            speed = -speed;
        }
        setLocation(getX() + speed, getY() );
    }
    private boolean atCenter;
    public void isMiddle()  
    {  
        if(getX() == centX && getY() == centY)
       
        {
            atCenter = true;
        }
        else
        {
            atCenter = false;
        }
        
    }

    public void playerOn() //WORKS 
    {
        Actor player = getOneIntersectingObject(Viking.class);
        if(this.isTouching(Viking.class) && player != null)
        {
            player.setLocation(player.getX() + speed, player.getY() );
        }
    }
    --------------------------
    
    
    
    
        
    -----------------------------------------------------------------------------------------------------------------------
        
     
public class LifeEater extends Actor
{
    Valkyrie player = new Valkyrie();
    /**
     * Act - do whatever the LifeEater wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        // Add your action code here.
        removeLife();
    }    

    
    
    
     

    
        public void removeLife()
    {
        int x = 1010;
        Actor life = getOneObjectAtOffset(x, 40, Life.class);
        if(player.hit() )
        {
            setLocation(life.getX(), life.getY() );
        }
        if( this.isTouching(Life.class) )
        {
            this.removeTouching(Life.class);
            x += 50;
        }
    }
