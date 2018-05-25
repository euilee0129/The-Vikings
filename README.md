# The-Vikings
Ap computer science final Greenfoot game

https://www.google.com/url?q=https://www.greenfoot.org/files/javadoc/&sa=D&ust=1527110096544000&usg=AFQjCNF1y1sWydbsJbhfEpWY5SDWRernxA
https://www.greenfoot.org/doc/tut-3
https://www.youtube.com/watch?v=RkVIVipKpfk

MyWorld:

/**
     * Constructor for objects of class MyWorld.
     * 
     */
    public MyWorld()
    {    
        // Create a new world with 600x400 cells with a cell size of 1x1 pixels.
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
        
        
        addObject( new Axe(), 100, getHeight()-100 );
        addObject( new Viking(), 200, getHeight()-200 );
        addObject( new Valkyrie(), 300, getHeight()-200 );
        
    }


Valkyrie:

private ArrayList<Actor> health = new ArrayList<Actor>();
    /**
     * Act - do whatever the Valkyrie wants to do. This method is called whenever
     * the 'Act' or 'Run' button gets pressed in the environment.
     */
    public void act() 
    {
        //Actor valkyrielife = new Life(); //1st heart
        health.add( getOneObjectAtOffset(1010, 40, Life.class ) );
        //Actor valkyrielife1 = new Life(); //2nd heart
        health.add( getOneObjectAtOffset(1060, 40, Life.class)  );
        if( foundAxe() )
        {
            takeDamage();
        }
         if( Greenfoot.isKeyDown("w") )
        {
            
            setRotation(-90);
            
            move(25);
        }
        if( Greenfoot.isKeyDown("a") )
        {
           
            setRotation(180);
            move(10);
        }
        if( Greenfoot.isKeyDown("d") )
        {

            setRotation(0);
            move(10);
        }
        if( Greenfoot.isKeyDown("s") )
        {

            setRotation(90);
            move(10);
        }
    } 
    
     public boolean foundAxe()
    {
        Actor axe = new Axe();
        if( isTouching( Axe.class )== true ) {
            return true;
        }
        else {
            return false;
        }
    }
    private int axeRemoved;
     public void takeDamage()
    {
        Actor axe = getOneObjectAtOffset(0, 0, Axe.class);
        if(axe != null) {
            //take damage and remove axe
            getWorld().removeObject(axe);
            axeRemoved = axeRemoved + 1;
            while( axeRemoved > 0 )
            {
                getWorld().removeObject(health.get(0) );
            }
        }
    }




