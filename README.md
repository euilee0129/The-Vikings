# The-Vikings
Ap computer science final Greenfoot game

P.S. using the blame button above the edit button lets u see time stamps of who did what to "blame" anyone lol
and using branches u can request a pull request, it's actually pretty easy


later find pictures that fit, u might have to edit sizes and stuff, and add sound effect
We'll start commenting once we finish

  
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
        
    }
    
    ----------------------------------------------
    
    private int verticalSpeed = 0;
    private int speed = 3;
    private int countJump;
    private boolean jumping = false;

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

    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
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
            fire();
        }
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
    private static final int AxeReloadTime = 5;        
    private int axeDelayCount;

    public void checkNewAxe()
    {
        fire();
        if( axeDelayCount > 0 )
        {
            axeDelayCount--;
            if( axeDelayCount == 0 )
            {
                fire();
            }
        }

    }

    public Viking()
    {
        axeDelayCount = 100;
    }

    /**
     * Throw axe when ready
     */
    private void fire() 
    {
        if ( axeDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            axeDelayCount = 0;
        }
    }
    
    ----------------------------
    
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
    
    
    ---------------------
    
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
    
    --------------
    
    Viking player1 = new Viking();
    Valkyrie player2 = new Valkyrie();
    ArrayList<Life> p2= new ArrayList<Life>();
    private int countDamage = 0 ;
    public Axe()
    {
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
        //checkNewAxe();
    } 

    private int takeDamage()
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

    public void damage()
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
    public void addedToWorld( World MyWorld )
    {
        posx = getX();
        posy = getY();

        ax= 0;
        ay= 10;

        vx= 50;
        vy=-70;

    }
    private int axeDelayCount = 0;
    public void newAxe()
    {
        axeDelayCount = 100;
    }
    private void checkNewAxe()
    {
        if( axeDelayCount > 0 )
        {
            axeDelayCount--;
            if( axeDelayCount == 0 )
            {
                newAxe();
            }
        }
    }
    public void arch()
    {
        posx += vx*dt+0.5*ax*dt*dt;
        posy += vy*dt+0.5*ay*dt*dt;

        vx+= ax*dt;
        vy+= ay*dt;

        setLocation((int)posx, (int)posy );
        if( isAtEdge() == true || isTouching( Valkyrie.class) == true )
        {
            World world = getWorld();
            world.removeObject( this);
        }
    }
    
    
    ------------
    updated vikgin
    
    
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

    public void moveForward()
    {
        setLocation(getX() + speed, getY() );
    }

    public void moveBack()
    {
        setLocation(getX() - speed, getY() );
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
            fire();
        }
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
    private static final int AxeReloadTime = 5;        
    private int axeDelayCount;

    public void checkNewAxe()
    {
        fire();
        if( axeDelayCount > 0 )
        {
            axeDelayCount--;
            if( axeDelayCount == 0 )
            {
                fire();
            }
        }

    }

    public Viking()
    {
        axeDelayCount = 100;
    }

    /**
     * Throw axe when ready
     */
    private void fire() 
    {
        if ( axeDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            axeDelayCount = 0;
        }
    }

    
}
  
-----------------

inside Viking class:

private static final int AxeReloadTime = 30;
    private int axeDelayCount = 100;
    /**
     * Throw axe everytime axeDelayCount reaches axeReloadTime
     */
    private void throwAxe() //for Marc, call throwAxe() inside checkkey for "l" 
    {
        if ( axeDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            axeDelayCount = 0;
        }
    }


