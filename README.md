# The-Vikings
Ap computer science final Greenfoot game

P.S. using the blame button above the edit button lets u see time stamps of who did what to "blame" anyone lol
and using branches u can request a pull request, it's actually pretty easy


I'll do world, life, and axe

you help me on life, find pictures that fit, u might have to edit sizes and stuff, and add sound effect
We'll start commenting once we finish

    MyWorld:
        
        /**
     * Constructor for objects of class MyWorld.
     * 
     */
    public MyWorld()
    {    
        // Create a new world with 1150 x 600 cells with a cell size of 1x1 pixels.
        super(1150, 600, 1);
        
        Actor vikinglife = new Life(); //1st heart
        this.addObject( vikinglife, 40, 40 );//differences of 50
        
        Actor vikinglife1 = new Life(); //2nd heart
        this.addObject( vikinglife1, 90, 40 );
        
        Actor vikinglife2 = new Life(); //3rd heart
        this.addObject( vikinglife2, 140, 40 );

        
        Actor valkyrielife = new Life(); //1st heart
        this.addObject( valkyrielife, 1010, 40 );//differences of 50
        
        Actor valkyrielife1 = new Life(); //2nd heart
        this.addObject( valkyrielife1, 1060, 40 );
        
        Actor valkyrielife2 = new Life(); //3rd heart
        this.addObject( valkyrielife2, 1110, 40 );
        
        Axe axe = new Axe();
        Sword sword = new Sword();
        addObject( axe, 200, getHeight()-100 );
        addObject( new Viking(), 200, getHeight()-200 );
        addObject( new Valkyrie(), 300, getHeight()-200 );
        //addObject( sword, 1000, getHeight()-200 );
        //addObject( new Ship(), getObjectsAt( 200, getHeight()-200, Viking.class).getX(), getObjectsAt( 200, getHeight()-200, Viking.class).getY(), Ship.class );
        addObject( new Ship(), 150, getHeight()-150 );
        
    }
        
        
        Life:
        
    private int heart;
    
    public Life()
    {
        heart = 1;//1 Hit point
    }
    
    /**
     * Act - do whatever the Life wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        remove();
    }
    public void remove()
    {
        World world = getWorld();
    }
        
        
        Axe:
            
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
        //throwAxe();
        arch();
        if( thrown == true )
        {
            Greenfoot.stop();
        }
        Greenfoot.start();
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

        vx= 100;
        vy=-60;

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
        else
        {
            if(isTouching(Valkyrie.class) )
            {
                damage();
                // Greenfoot.stop();

                // }
                // else
                // {
                // Greenfoot.start();
            }
        }
    }
    public class Ocean extends Actor
{
    private World world = getWorld();
    /**
     * Act - do whatever the Ocean wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {   
        remove();
    }    

    public void remove()
    { 
        if(this.isTouching(null) )
        {
            this.removeTouching(null);
        }
    }
}
        
    -----------------------------------------------------------------------------------------------------------------------
        
         private static final int AxeReloadTime = 5;        
    private int reloadDelayCount;
    public Viking()
    {
        reloadDelayCount = 5;
    }
    /**
     * Throw axe when ready
     */
    private void fire() 
    {
        if (reloadDelayCount >= AxeReloadTime) 
        {
            Axe axe = new Axe();
            getWorld().addObject( axe, getX(), getY() );
            axe.arch();
            reloadDelayCount = 0;
        }
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
        if(player.hit() )
        {
            move(50);
        }
        if( this.isTouching(Life.class) )
        {
            this.removeTouching(Life.class);
        }
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
        int x = 1010;
        Actor life = getOneObjectAtOffset(x, 40, Life.class);
        if(player.hit() )
        {
            setLocation(life.getX(), life.getY() );
            x += 50;
        }
        if( this.isTouching(Life.class) )
        {
            this.removeTouching(Life.class);
        }
    }
