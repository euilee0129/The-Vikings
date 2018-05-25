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
    public void throwAxe()
    {
        if( this.isTouching( Valkyrie.class ) == false )
        {
            if( Greenfoot.isKeyDown("l") )
            {
                if( thrown == false )
                {
                    setRotation(90);
                    setLocation(player2.getX(), player2.getY());
                    thrown = true;
                }
                else
                {
                    setLocation( player1.getX(), player1.getY() );
                }
            }
        }
        else
        {
            damage();
        }

    }

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
        if( Greenfoot.isKeyDown("l") )
        {
            posx += vx*dt+0.5*ax*dt*dt;
            posy += vy*dt+0.5*ay*dt*dt;

            vx+= ax*dt;
            vy+= ay*dt;

            setLocation((int)posx, (int)posy );
            if( isAtEdge() == true && isTouching( Valkyrie.class) == false )
            {
                setLocation(player1.getX()+10, player1.getY() );
            }
            else
            {
                if(isTouching(Valkyrie.class) )
                {
                    damage();
                    Greenfoot.stop();

                }
                else
                {
                    Greenfoot.start();
                }
            }
        }
    }
    
    Viking:
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
    Ship:


    public class Ship extends Actor
    {
      private int speed = 3;
       /**
       * Act - do whatever the Ship wants to do. This method is called whenever
       * the 'Act' or 'Run' button gets pressed in the environment.
       */
       public void act() 
      {
         move();
         playerOn();
     }    
       public void move()
     {
         if(isAtEdge() )
         {
                speed = -speed;
        }
        setLocation(getX() + speed, getY() );
     }
        public void playerOn() //WORKS 
     {
        Actor player = getOneIntersectingObject(Viking.class);
        if(this.isTouching(Viking.class) && player != null)
        {
            player.setLocation(player.getX() + speed, player.getY() );
        }
    }
}
